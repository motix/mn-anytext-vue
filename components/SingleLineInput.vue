<template>
    <div></div>
</template>

<script>
    import { SingleLineInput } from 'mn-anytext';

    export default {
        props: ['value', 'placeholder', 'placeholder-css-class'],
        data() {
            return {
                input: null
            };
        },
        watch: {
            value(newValue) {
                this.input.value = newValue ;
            }
        },
        mounted() {
            var that = this;
            this.input = new SingleLineInput(this.$el, {
                value: this.value,
                placeholder: {
                    emptyInputCssClass: this.placeholderCssClass,
                    emptyInputText: this.placeholder
                },
                onChange() {
                    that.$emit('input', that.input.value);
                }
            });
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