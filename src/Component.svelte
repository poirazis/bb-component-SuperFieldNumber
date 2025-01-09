<script>
  import { getContext, onDestroy } from "svelte";
  import CellNumber from "../../bb_super_components_shared/src/lib/SuperTableCells/CellNumber.svelte";
  import SuperButton from "../../bb_super_components_shared/src/lib/SuperButton/SuperButton.svelte";
  import SuperFieldLabel from "../../bb_super_components_shared/src/lib/SuperFieldLabel/SuperFieldLabel.svelte";
  import "../../bb_super_components_shared/src/lib/SuperFieldsCommon.css";

  const { styleable, enrichButtonActions, Provider } = getContext("sdk");
  const component = getContext("component");
  const allContext = getContext("context");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupDisabled = getContext("field-group-disabled");
  const groupColumns = getContext("field-group-columns");
  const formApi = formContext?.formApi;

  export let field;
  export let controlType;
  export let role = "formInput";
  export let labelPosition = "fieldGroup";

  export let buttons = [];

  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let validation;
  export let helpText;
  export let align;

  export let template;

  export let onChange;
  export let debounced;
  export let debounceDelay;

  export let icon;
  export let clearValueIcon;
  export let showDirty;
  export let autofocus;

  let formField;
  let formStep;
  let fieldState;
  let fieldApi;
  let fieldSchema;
  let value;

  $: formStep = formStepContext ? $formStepContext || 1 : 1;
  $: labelPos = label
    ? groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition
    : false;

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
    align,
    error: fieldState?.error,
    debounce: debounced ? debounceDelay : false,
    clearValueIcon,
    role,
    showDirty,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      "flex-direction": labelPos == "left" ? "row" : "column",
      "align-items": "stretch",
      gap: labelPos == "left" ? "0.5rem" : "0rem",
      "grid-column": span < 7 ? "span " + span : "span " + groupColumns * 6,
      "--label-width":
        labelPos == "left" ? (labelWidth ? labelWidth : "6rem") : "auto",
    },
  };

  const handleChange = (newValue) => {
    value = newValue;
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
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div use:styleable={$component.styles}>
  <Provider data={{ value }} />
  <div
    class="superField"
    class:left-label={labelPos == "left"}
    class:multirow={controlType != "select"}
  >
    <SuperFieldLabel
      {labelPos}
      {labelWidth}
      {label}
      {helpText}
      error={fieldState?.error}
    />

    <div class="inline-cells">
      <CellNumber
        {autofocus}
        {cellOptions}
        {value}
        {fieldSchema}
        on:change={(e) => handleChange(e.detail)}
      />

      {#if buttons?.length}
        <div class="inline-buttons">
          {#each buttons as { icon, text, onClick, quiet, type, size }}
            <SuperButton
              {icon}
              {quiet}
              disabled={disabled || fieldState?.disabled}
              {size}
              {type}
              {text}
              on:click={enrichButtonActions(onClick, $allContext)}
            />
          {/each}
        </div>
      {/if}
    </div>
  </div>
</div>
