# MacOS Traffic Lights Vue Component

Self-contained Vue 3 component that renders the macOS window controls (close, minimize, maximize) with their hover / active / unfocused states.

Based on <a href="https://github.com/aw3r1se/macOS-traffic-lights">aw3r1se/macOS-traffic-lights</a> — if you just need the SVG files, check that first.

## 🔧 Installation

```sh
npm i macos-traffic-lights-vue
```

The component ships as a single `.vue` source file, so your project needs a Vue SFC compiler (Vite, vue-cli, etc.). No CSS framework is required — styling is scoped to the component.

## ✏️ Usage

```vue
<script setup>
  import TrafficLights from 'macos-traffic-lights-vue';

  const handleClose = () => { /* ... */ };
  const handleMinimize = () => { /* ... */ };
  const handleMaximize = () => { /* ... */ };
</script>

<template>
  <TrafficLights
      @close="handleClose"
      @minimize="handleMinimize"
      @maximize="handleMaximize"
  />
</template>
```

### Props

| Prop   | Type             | Default | Description                     |
|--------|------------------|---------|---------------------------------|
| `size` | `Number\|String` | `12`    | Width/height of each button, px |

### Events

| Event       | Fired when                  |
|-------------|-----------------------------|
| `close`     | The red button is clicked   |
| `minimize`  | The yellow button is clicked|
| `maximize`  | The green button is clicked |

### Focus / unfocus

Use a template ref to toggle the unfocused (dimmed) appearance — e.g. when the
window loses focus:

```vue
<script setup>
  import { ref } from 'vue';
  import TrafficLights from 'macos-traffic-lights-vue';

  const trafficLights = ref();

  const onWindowFocus = () => trafficLights.value.focus();
  const onWindowBlur = () => trafficLights.value.unfocus();
</script>

<template>
  <TrafficLights
      ref="trafficLights"
      @close="handleClose"
      @minimize="handleMinimize"
      @maximize="handleMaximize"
  />
</template>
```

| Method      | Description                                  |
|-------------|----------------------------------------------|
| `focus()`   | Restores the default (focused) appearance    |
| `unfocus()` | Dims all buttons to the unfocused appearance |

## 🤝 Contributing

If you want to add or improve something — you are welcome:

* Fork → Branch → Commit with `feat:` / `fix:` prefix
* Test locally
* Open a pull request
