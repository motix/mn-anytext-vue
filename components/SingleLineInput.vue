<template>
    <div></div>
</template>

<script>
    import { SingleLineInput } from 'mn-anytext';

    export default {
        props: {
            value: null,
            placeholder: String,
            'placeholder-css-class': String,
            'placeholder-align': String,
            wrapping: {
                type: Boolean,
                default: true
            },
            prefix: String,
            suffix: String
        },
        data() {
            return {
                input: null,
                converter: {
                    toNumber(args) {
                        if (args.input === null || args.input === '') {
                            args.result = '';
                            return;
                        }
                        args.result = Number(args.input);
                        if (isNaN(args.result)) {
                            args.result = '';
                        }
                    },

                    fromNumber(args) {
                        if (args.output === null || args.output === '') {
                            args.result = null;
                            return;
                        }
                        args.result = Number(args.output);
                        if (isNaN(args.result)) {
                            args.result = '';
                        }
                    },
                    toPercent(args) {
                        if (args.input === null || args.input === '') {
                            args.result = '';
                            return;
                        }
                        args.result = Number((Math.round(args.input * 10000) / 100).toFixed(2));
                        if (isNaN(args.result)) {
                            args.result = '';
                        }
                    },

                    fromPercent(args) {
                        if (args.output === null || args.output === '') {
                            args.result = null;
                            return;
                        }
                        args.result = Number((Math.round(args.output * 100) / 10000).toFixed(4));
                        if (isNaN(args.result)) {
                            args.result = null;
                        }
                    }
                }
            };
        },
        watch: {
            value(newValue) {
                const args = { input: newValue, result: newValue };
                this.$emit('convert-input', this, args);
                this.input.value = args.result;
            }
        },
        mounted() {
            const that = this;

            const args = { input: this.value, result: this.value };
            this.$emit('convert-input', this, args);

            this.input = new SingleLineInput(this.$el, {
                value: args.result,
                placeholder: {
                    emptyInputCssClass: this.placeholderCssClass,
                    emptyInputText: this.placeholder,
                    align: this.placeholderAlign
                },
                wrapping: this.wrapping,
                prefix: this.prefix,
                suffix: this.suffix,
                onChange() {
                    const args = { output: that.input.value, result: that.input.value };
                    that.$emit('convert-output', that, args);
                    that.$emit('input', args.result);
                }
            });
        }
    };
</script>

<style>
    [data-prefix-text]:first-child::before {
        content: attr(data-prefix-text);
        float: left;
    }

    [data-suffix-text]:first-child::before {
        content: attr(data-suffix-text);
        float: right;
    }

    [data-empty-input-text] {
        &:first-child::before {
            content: attr(data-empty-input-text);
            height: 0;
        }
        
        &[data-empty-input-align=left]:first-child::before {
            float: left;
        }
        
        &[data-empty-input-align=right]:first-child::before {
            float: right;
        }
    }
</style>