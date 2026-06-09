<script setup>
import { reactive, ref } from 'vue';

import * as icons from 'macos-traffic-lights';

const BUTTONS = ['close', 'minimize', 'maximize'];

// Build the per-button icon set from the upstream exports so the three
// buttons stay in sync (e.g. icons.closeDefault, icons.closeHover, ...).
const iconsByButton = Object.fromEntries(
    BUTTONS.map((name) => [
        name,
        {
            default: icons[`${name}Default`],
            hover: icons[`${name}Hover`],
            active: icons[`${name}Active`],
            unfocused: icons.unfocused,
        },
    ]),
);

defineProps({
    // Width/height of each button, in pixels.
    size: {
        type: [Number, String],
        default: 12,
    },
});

const emit = defineEmits(['close', 'minimize', 'maximize']);

// The state the buttons return to when the pointer leaves the group.
const baseState = ref('default');
const buttonStates = reactive({
    close: 'default',
    minimize: 'default',
    maximize: 'default',
});

const iconSrc = (name) => iconsByButton[name][buttonStates[name]];

const onGroupEnter = () => {
    BUTTONS.forEach((name) => {
        if (buttonStates[name] !== 'active') {
            buttonStates[name] = 'hover';
        }
    });
};

const onGroupLeave = () => {
    BUTTONS.forEach((name) => {
        buttonStates[name] = baseState.value;
    });
};

const onPressStart = (name) => {
    onGroupEnter();
    buttonStates[name] = 'active';
};

const onPressEnd = (name) => {
    buttonStates[name] = 'hover';
};

const onClick = (name) => {
    if (baseState.value === 'unfocused') {
        baseState.value = 'default';
    }
    emit(name);
};

const setBaseState = (state) => {
    baseState.value = state;
    BUTTONS.forEach((name) => {
        buttonStates[name] = state;
    });
};

const focus = () => setBaseState('default');
const unfocus = () => setBaseState('unfocused');

defineExpose({
    focus,
    unfocus,
});
</script>

<template>
    <div
        class="macos-traffic-lights"
        @mouseenter="onGroupEnter"
        @mouseleave="onGroupLeave"
    >
        <img
            v-for="name in BUTTONS"
            :key="name"
            :src="iconSrc(name)"
            :alt="name"
            :width="size"
            :height="size"
            class="macos-traffic-lights__button"
            draggable="false"
            @mousedown="onPressStart(name)"
            @mouseup="onPressEnd(name)"
            @click="onClick(name)"
        />
    </div>
</template>

<style scoped>
.macos-traffic-lights {
    display: inline-flex;
    align-items: center;
    gap: 8px;
}

.macos-traffic-lights__button {
    cursor: default;
    user-select: none;
}
</style>
