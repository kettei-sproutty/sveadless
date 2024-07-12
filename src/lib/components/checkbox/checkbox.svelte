<script lang="ts">
  import type { HTMLInputAttributes } from 'svelte/elements';

  type OverridenInputAttributes = 'type' | 'children';

  type InputProps = Omit<HTMLInputAttributes, OverridenInputAttributes> & {
    type: "checkbox" | "hidden",
  };

  let { type, onmouseenter, onmouseleave, onfocus, onblur, ...props }: InputProps = $props();

  let isHover = $state<true | undefined>();
  let isFocus = $state<true | undefined>();
  let isChecked = $state<true | undefined>();

  const onMouseEnter: typeof onmouseenter = (event) => {
  isHover = true;
  onmouseenter?.(event);
}

const onMouseLeave: typeof onmouseleave = (event) => {
  isHover = void 0;
  onmouseleave?.(event);
}

const onFocus: typeof onfocus = (event) => {
  isFocus = true;
  onfocus?.(event);
}

const onBlur: typeof onblur = (event) => {
  isFocus = void 0;
  onblur?.(event);
}

</script>

<input
  {type}
  onmouseenter={onMouseEnter}
  onmouseleave={onMouseLeave}
  onfocus={onFocus}
  onblur={onBlur}
  data-hover={isHover}
  data-focus={isFocus}
  data-autofocus={props.autofocus ? true : void 0}
  aria-disabled={props.disabled ? true : void 0}
  {...props}
/>
