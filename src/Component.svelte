<script>
  import { getContext, onDestroy } from "svelte";
  import CellNumber from "../../bb_super_components_shared/src/lib/SuperTableCells/CellNumber.svelte";
  import "../../bb_super_components_shared/src/lib/SuperFieldsCommon.css";

  const { styleable, Provider, BlockComponent, Block } = getContext("sdk");
  const component = getContext("component");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const labelPos = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupDisabled = getContext("field-group-disabled");
  const formApi = formContext?.formApi;

  export let field;
  export let controlType;

  export let customButtons;
  export let buttons = [];
  export let buttonsQuiet;

  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let validation;

  export let template;

  export let onChange;
  export let debounced;
  export let debounceDelay;

  export let icon;
  export let clearValueIcon;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;
  let value;
  let cellState;

  $: formStep = formStepContext ? $formStepContext || 1 : 1;

  $: formField = formApi?.registerField(
    field,
    "number",
    defaultValue,
    disabled,
    readonly,
    validation,
    formStep
  );

  $: unsubscribe = formField?.subscribe((value) => {
    fieldState = value?.fieldState;
    fieldApi = value?.fieldApi;
    fieldSchema = value?.fieldSchema;
  });

  $: value = fieldState?.value ? fieldState.value : defaultValue;

  $: cellOptions = {
    placeholder,
    defaultValue,
    template,
    disabled: disabled || groupDisabled || fieldState?.disabled,
    readonly: readonly || fieldState?.readonly,
    icon,
    error: fieldState?.error,
    debounce: debounced ? debounceDelay : false,
    clearValueIcon,
    role: "formInput",
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      "flex-direction": labelPos == "left" ? "row" : "column",
      gap: labelPos == "left" ? "0.5rem" : "0rem",
      "grid-column": labelPos ? "span " + span : null,
      "--label-width":
        labelPos == "left" ? (labelWidth ? labelWidth : "6rem") : "auto",
    },
  };

  const handleChange = (newValue) => {
    onChange?.({ value: newValue });
    fieldApi?.setValue(newValue);
  };

  onDestroy(() => {
    fieldApi?.deregister();
    unsubscribe?.();
  });
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<Block>
  <div class="superField" use:styleable={$component.styles}>
    {#if label}
      <label for="superCell" class="superlabel" class:left={labelPos == "left"}>
        {label}
        {#if fieldState.error}
          <div class="error" class:left={labelPos == "left"}>
            <span>{fieldState.error}</span>
          </div>
        {/if}
      </label>
    {/if}

    <div class="inline-cells">
      <CellNumber
        bind:cellState
        {cellOptions}
        {value}
        {fieldSchema}
        on:change={(e) => handleChange(e.detail)}
        on:blur={cellState.lostFocus}
      />

      {#if customButtons && buttons?.length}
        <div
          class="spectrum-ActionGroup spectrum-ActionGroup--compact spectrum-ActionGroup--sizeM"
          class:spectrum-ActionGroup--quiet={buttonsQuiet}
        >
          <Provider data={{ value }}>
            {#each buttons as { text, onClick, icon, disabled, quiet, type }}
              <BlockComponent
                type="plugin/bb-component-SuperButton"
                props={{
                  size: "M",
                  text,
                  icon,
                  disabled,
                  quiet,
                  type,
                  onClick,
                }}
              ></BlockComponent>
            {/each}
          </Provider>
        </div>
      {/if}
    </div>
  </div>
</Block>
