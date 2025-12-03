<script>
  import { getContext, onDestroy } from "svelte";
  import {
    CellNumber,
    SuperButton,
    SuperField,
  } from "@poirazis/supercomponents-shared";

  const { styleable, enrichButtonActions, Provider, builderStore } =
    getContext("sdk");
  const component = getContext("component");
  const allContext = getContext("context");

  const formContext = getContext("form");
  const formStepContext = getContext("form-step");
  const groupLabelPosition = getContext("field-group");
  const labelWidth = getContext("field-group-label-width");
  const groupDisabled = getContext("field-group-disabled");
  const groupColumns = getContext("field-group-columns");
  const formApi = formContext?.formApi;

  export let field = "Number Field";
  export let role = "formInput";
  export let labelPosition = "fieldGroup";

  export let buttons = [];

  export let label;
  export let span = 6;
  export let placeholder;
  export let defaultValue;
  export let disabled;
  export let readonly;
  export let decimals = 0;
  export let validation;
  export let invisible = false;
  export let showStepper = true;
  export let stepSize = 1;
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

  $: labelPos =
    groupLabelPosition && labelPosition == "fieldGroup"
      ? groupLabelPosition
      : labelPosition;

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

  $: value = fieldState?.value;
  $: setDefaultValue(defaultValue);
  $: error = fieldState?.error;

  $: cellOptions = {
    placeholder: placeholder || field,
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
    showStepper,
    stepSize,
    decimals,
  };

  $: $component.styles = {
    ...$component.styles,
    normal: {
      ...$component.styles.normal,
      display:
        invisible && !$builderStore.inBuilder
          ? "none"
          : $component.styles.normal.display,
      opacity: invisible && $builderStore.inBuilder ? 0.6 : 1,
      "grid-column": groupColumns ? `span ${span}` : "span 1",
      overflow: "hidden",
    },
  };

  const handleChange = (newValue) => {
    value = newValue;
    onChange?.({ value: newValue });
    fieldApi?.setValue(newValue);
  };

  const setDefaultValue = (val) => {
    if (val == null || val == "") {
      return;
    } else {
      let num = Number(val);
      value = isNaN(num) ? null : num;
    }
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
  <SuperField {labelPos} {labelWidth} {field} {label} {error} {helpText}>
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
            onClick={enrichButtonActions(onClick, $allContext)}
          />
        {/each}
      </div>
    {/if}
  </SuperField>
</div>
