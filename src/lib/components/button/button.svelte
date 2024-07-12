<script lang="ts">
  import type { Snippet } from "svelte";
  import type { HTMLButtonAttributes } from "svelte/elements";

  type OverridenButtonAttributes = 'type';

  type ButtonProps = Omit<HTMLButtonAttributes, OverridenButtonAttributes> & {
    children: Snippet;
    /**
     * The default button `type` is `submit`.
     * It is always better to specify the `type` attribute.
    */
    type: 'button' | 'submit' | 'reset';
  }

  let {
    onmouseenter = $bindable(),
    onmouseleave = $bindable(),
    onmousedown = $bindable(),
    onmouseup = $bindable(),
    onfocus = $bindable(),
    onblur = $bindable(),
    children,
    ...props
  }: ButtonProps = $props();

  let isHover = $state<true | undefined>();
  let isActive = $state<true | undefined>();
  let isFocus = $state<true | undefined>();

  const onMouseEnter: typeof onmouseenter = (event) => {
    isHover = true;
    onmouseenter?.(event);
  }

  const onMouseLeave: typeof onmouseleave = (event) => {
    isHover = void 0;
    isActive = void 0;
    onmouseleave?.(event);
  }

  const onMouseDown: typeof onmousedown = (event) => {
    isActive = true;
    onmousedown?.(event);
  }

  const onMouseUp: typeof onmouseup = (event) => {
    isActive = void 0;
    onmouseup?.(event);
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

<!--
@component
The `Button` component is a simple button element with some extra features.
It `requires` a `type` attribute to be specified.
- Provides `data-attributes` for `hover`, `active`, and `focus` states.
- Provides `aria-disabled` attribute when the `disabled` prop is `true`.
- Provides `data-autofocus` attribute when the `autofocus` prop is `true`.
- Forwards all the `HTMLButtonAttributes` to the button element.
- Usage:
```tsx
<Button type="button">Button</Button>
```
- Rendered HTML:

without `hover` `active` `focus` states:
```html
<button type="button">Button</button>
```
with `hover` `active` `focus` states:
```html
<button data-hover data-active data-focus type="button">Button</button>
```
 -->
<button
  onmouseenter={onMouseEnter}
  onmouseleave={onMouseLeave}
  onmousedown={onMouseDown}
  onmouseup={onMouseUp}
  onfocus={onFocus}
  onblur={onBlur}
  data-hover={isHover}
  data-active={isActive}
  data-focus={isFocus}
  data-autofocus={props.autofocus ? true : void 0}
  aria-disabled={props.disabled ? true : void 0}
  {...props}
>
  {@render children()}
</button>
