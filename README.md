# Tiny printf/sprintf implementation

This is a small implementation of printf/sprintf family of functions based on Kustaa Nyholm printf.c. It has built-in conversion code that can optionally be used or not, but it is not a standalone implementation. It depends on some libc functions and headers.

- _utoac (if TFP_PRINTF_USE_BUILTINS is not defined)
- _ultoac (if TFP_PRINTF_USE_BUILTINS is not defined)
- _ulltoac (if TFP_PRINTF_USE_BUILTINS is not defined)
- stdio.h (if TFP_PRINTF_USE_BUILTINS is not defined)
- stdlib.h (if TFP_PRINTF_USE_BUILTINS is not defined)
- stdint.h
- limits.h
- stdarg.h


### Compile time options

| Option name                            | Description  |
|----------------------------------------|--------------|
| TFP_PRINTF_USE_BUILTINS | Use built-in conversion code instead of MCU-LIBC functions. This should probably be defined. |
| TFP_PRINTF_USE_SHIFT_ADD | Use shift-and-add multiplication instead of standard multiplication. This can be handy on some low end MCUs without hw multiplier. |
| TFP_PRINTF_ENABLE_LONGLONG | Enable support for 64.bit long long int. |
| TFP_PRINTF_ENABLE_LONG | Enable support for 32.bit long int. |
| PRINTF_USE_TFP | Enable standard libc function names as aliases to tfp_* functions. |


### Implemented functions

All functions have the same signature as in the libc (except the msp_ prefix and  non-standard function init_msp_printf):
```
void init_tfp_printf(int (*putf) (int));
void tfp_format(void*, void (*)(void*, char), const char*, va_list);
int tfp_vsnprintf (char *buf, size_t size, const char *fmt, va_list argp);
int tfp_vsprintf (char *buf, const char *fmt, va_list argp);
int tfp_vprintf (const char *fmt, va_list argp);
int tfp_puts (const char *str);
int tfp_uprintf(int (*outf)(int), const char *fmt, ...);
int tfp_printf(const char *fmt, ...);
int tfp_sprintf(char* s, const char *fmt, ...);
int tfp_snprintf(char* s, size_t size, const char *fmt, ...);
```
Note that you first have to call `init_tfp_printf()` to set I/O callback function like `putchar()` that will be used for actual printing.

Note also that returned value is always 0 regardless of number of printed characters.

### Standard libc function names

If you define compile time symbol PRINTF_USE_TFP all functions will we visible as the standard library's names, without tfp_ prefix.


