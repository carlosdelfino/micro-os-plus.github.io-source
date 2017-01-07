---
layout: page
lang: pt
permalink: /pt/support/faq/
title: FAQ
author: Liviu Ionescu
translator: Carlos Delfino

date: 2015-09-11 20:28:00 +0300
last_modified_at: 2017-01-07 14:30:00 +300

---
---
{% comment %}
Start translate at: 2017-01-07 14:30:00 +300

Base Commit: 
 - 8c25c851aedaace391a0556777343f0b674e0558
{% endcomment %}
## Porque não posso depurar passo a passo na função sprintf()?

Depurar passo a passo em alguma função requer a presença de informação de depuração. Normalmente, a implementação da família de funções `printf() está na biblioteca padrão do C (Standard C Library), que não incluem informações de depuração.

Porém, para configurações comuns do GCC, usando biblioteca open source **newlib**, o código fonte de algumas funções estão disponíveis; ele pode ser (temporariamente) adicionada a um projeto e compilada com informações de depuração.

Os fontes da [newlib](https://sourceware.org/newlib/) podem ser clonados de:

```
git clone git://sourceware.org/git/newlib-cygwin.git
```

Crie uma subpasta na pasta dos fontes de seu projeto (por exemplo `newlib-libc`) e copie os arquivos desejados de `newlib-cygwin.git/newlib/libc/stdio`. Você pode também precisar copiar algums cmbeçalhos adicionadis, das pastas relacionadas.

Por exemplo, para depurar passo a passo em `snprintf()`e `trace::printf()`, os seguintes arquivos são necessários:

```
$ tree newlib-libc
newlib-libc
├── locale
│   └── setlocale.h
├── stdio
│   ├── dtoa.c
│   ├── fvwrite.h
│   ├── local.h
│   ├── mprec.c
│   ├── mprec.h
│   ├── nano-vfprintf.c
│   ├── nano-vfprintf_float.c
│   ├── nano-vfprintf_local.h
│   ├── snprintf.c
│   ├── vfieeefp.h
│   └── vsnprintf.c
└── stdlib
    └── local.h

3 directorios, 13 arquivos
```

Para a compilação ser um sucesso, pode ser necessário definir algumas macros; para a arvore acima, as seguintes definições são necessárias:

```
STRING_ONLY
```

Por favor, observe que os fontes de newlib estão longe de estarem livres de _warning_, e é recomendado disabilitar alguma verficação de `warning` para a pasta `newlib-libc`.

Se a nano versão do newlib é usada (como no exemplo acima), e impressão de _floats_ é desejada, as rotinas de conversão de floats precisam ser explicitamente inclusas, atráves da adição `-u _printf_float`para o linker.


