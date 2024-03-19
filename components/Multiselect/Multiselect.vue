<template>
<div
    class="form-group"
    :class="{
        'input-group-focus': focused,
        'has-danger': error,
        'has-success': !error && touched,
        'has-label': label,
        'has-icon': hasIcon,
    }"
>
    <slot name="label">
    <label v-if="label"> {{ label }} {{ required ? '*' : '' }} </label>
    </slot>
    <div class="mb-0" :class="{'input-group': hasIcon}">
        <slot name="addonLeft">
            <span v-if="addonLeftIcon" class="input-group-prepend">
            <div class="input-group-text"><i :class="addonLeftIcon"></i></div>
            </span>
        </slot>
        <slot>
            <div @click="openDropDown" class="form-control hover-pointer">

            </div>
        </slot>
        <!-- CONTAINER FOR RENDERING THE OPTIONS: -->
        <div v-if="hasDropdown" class="w-100 h-0">
            <input
                :value="value"
                v-bind="$attrs"
                v-on="listeners"
                placeholder="Digite sua busca aqui.."
                class="form-control bg-white border-0 rounded-0 position-relative text-dark z-index-2"
                aria-describedby="addon-right addon-left "
            />
            <div class="w-100 h-200px overflow-auto position-relative top-0 end-0 bg-white z-index-2 hover-pointer">
                <span v-for="(item, index) in items" :key="index" :class="item.isSelected ? 'text-white bg-green' : 'text-black hover-text-white hover-bg-dark'" class="d-flex w-100">{{ item.label }}</span>
            </div>
        </div>
        <!-- OVERLAY TO CLOSE THE DROPDOWN -->
        <div v-if="hasDropdown" class="w-100 h-0">
            <div @click="closeDropDown" class="position-fixed top-0 left-0 w-100 h-100 z-index-1"></div>
        </div>
        <slot name="addonRight">
            <span v-if="addonRightIcon" class="input-group-append">
            <div class="input-group-text"><i :class="addonRightIcon"></i></div>
            </span>
        </slot>
    </div>

    <slot name="error" v-if="error || $slots.error">
        <label class="error">{{ error }}</label>
    </slot>
    <slot name="helperText"></slot>
</div>
</template>
<script>
export default {
inheritAttrs: false,
name: 'multiselect',
props: {
    required: Boolean,
    label: {
        type: String,
        description: 'Input label'
    },
    error: {
        type: String,
        description: 'Input error',
        default: ''
    },
    value: {
        type: [String, Number],
        description: 'Input value'
    },
    addonRightIcon: {
        type: String,
        description: 'Input icon on the right'
    },
    addonLeftIcon: {
        type: String,
        description: 'Input icon on the left'
    }
},
model: {
    prop: 'value',
    event: 'input'
},
data() {
    return {
    focused: false,
    touched: false,
    hasDropdown:false,
    items:[
        {
            label:'teste',
            isSelected:false,
            id:1
        },
        {
            label:'teste',
            isSelected:false,
            id:2
        },
        {
            label:'teste',
            isSelected:false,
            id:3
        },
        {
            label:'teste',
            isSelected:false,
            id:4
        },
        {
            label:'teste',
            isSelected:false,
            id:5
        },
        {
            label:'teste',
            isSelected:false,
            id:6
        }
    ]
    };
},
computed: {
    hasIcon() {
    return this.hasLeftAddon || this.hasRightAddon
    },
    hasLeftAddon() {
    const { addonLeft } = this.$slots;
    return (
        addonLeft !== undefined ||
        this.addonLeftIcon !== undefined
    );
    },
    hasRightAddon() {
    const { addonRight } = this.$slots;
    return (
        addonRight !== undefined ||
        this.addonRightIcon !== undefined
    );
    },
    listeners() {
    return {
        ...this.$listeners,
        input: this.onInput,
        blur: this.onBlur,
        focus: this.onFocus
    };
    }
},
methods: {
    onInput(evt) {
        if (!this.touched) {
            this.touched = true;
        }
        this.$emit('input', evt.target.value);
    },
    onFocus(evt) {
        this.focused = true;
        this.$emit('focus', evt)
    },
    onBlur(evt) {
    this.focused = false;
    this.$emit('blur', evt)
    },
    openDropDown(){
        this.hasDropdown = true;
    },
    closeDropDown(){
        this.hasDropdown = false;
    }
}
};
</script>
<style></style>