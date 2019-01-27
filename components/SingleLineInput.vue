<template>
    <div></div>
</template>

<script>
    import { SingleLineInputView } from 'mn-anytext';

    export default {
        props: ['value', 'placeholder', 'placeholder-css-class'],
        data() {
            return {
                view: null
            };
        },
        watch: {
            value(newValue) {
                const value = this.getValue();
                if (newValue === value) {
                    return;
                }

                this.view.updateValue(newValue == null ? newValue : String(newValue));
            }
        },
        mounted() {
            var that = this;
            this.view = new SingleLineInputView(this.$el, {
                value: this.value == null ? this.value : String(this.value),
                placeholder: {
                    emptyInputCssClass: this.placeholderCssClass,
                    emptyInputText: this.placeholder
                },
                onChange() {
                    that.$emit('input', that.view.state.doc.textContent);
                }
            });
        },
        methods: {
            getValue() {
                return this.view.state.doc.textContent;
            }
        }
    };
</script>

<style>
    @supports (-moz-appearance: none) {
        .ProseMirror[contenteditable] {
            white-space: pre-wrap;
        }
    }

    [data-empty-input-text]:first-child::before {
        content: attr(data-empty-input-text);
        height: 0;
        float: left;
    }
</style>