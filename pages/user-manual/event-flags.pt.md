---
layout: page
lang: pt
permalink: /pt/user-manual/event-flags/
title: Sinalizadores de Eventos
author: Liviu Ionescu
translator: Carlos Delfino

date: 2016-07-15 10:09:00 +0300
last_updated_at:  2017-01-08 09:51:00 +0300

---
{% comment %}

start_translate_at:  2017-01-08 09:51:00 +0300

 Base Commit: 
 - 0ff10d71be7c5551398ddf85685efb53fa6a37e7

{% endcomment %}

## Visão Geral

Da mesma forma que semáforos, eventos de _threads_ são mecanismos básicos de sincronização do µOS++.

Um sinalizador de evento pode ser considerado como um semáforo binário, que pode ser postado de uma _thread_ ou _ISR_.

O valor adicionado dos sinalizadores de eventos consite em seu número: Sinalizadores de eventos chegam em grupos, e _threads_ podem ser sincronizadas em um certo número de sinalizadores em um grupo (definido por uma mascara). Uma thread pode experar que todos os sinalizadores em um conjunto tenha ocorrido (sincronização conjuntiva, AND lógico), ou algum sinalizador em um conjunto (sincronização disjuntiva, OR lógico).

<div style="text-align:center">
<img src="{{ site.baseurl }}/assets/images/2016/event-flags.png" />
<p>Event flags</p>
</div>

Os sinalizadores de eventos consistem de uma série de bits, basados no tamanho da palavra da plataforma (32-bits para Cortex-M). O valor inicial de um sinalizador de eventos é zero (nenhuma das sinalizações foi lançada).

## Criando Sinalizadores de Eventos

Quando usado para sincronizar threads com ISRs, A forma mais fácil de acessar o sinalizador de eventos é quando ele são criados como objetos globais.

No C++ os sinalizadores de evento globais são criados e inicializados por um mecanismo global de construção estática.


``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Create a global event flags object instance.
event_flags ev { "ev" };

int
os_main (int argc, char* argv[])
{
  // ...

  // Not much to do, the event flags were created by the static
  // constructors, before entering main().

  // ...

  return 0;
}

// All event flags are automatically destroyed if os_main() returns.

```

Um exemplo similar, mas escrito em C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

// Global static storage for the event flags object instance.
os_evflags_t ev;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a global event flags object instance.
  os_evflags_create(&ev, "ev", NULL);

  // ...

  // For completeness, destroy the event flags.
  os_evflags_destroy(&ev);

  return 0;
}
```

Em C++, se é necessário controlar o momento quando instâncias de objetos globais são criados, é possível alocar separadamente o armazenamento como variáveis globais, então usar o operador `new` para inicializa-lo.

``` c++
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Global static storage for the event flags object instance.
// This storage is set to 0 as any uninitialised variable.
std::aligned_storage<sizeof(event_flags), alignof(event_flags)>::type ev1;

int
os_main (int argc, char* argv[])
{
  // ...

  // Use placement new, to explicitly call the constructor
  // and initialise the event flags.
  // Create a global event flags object instance.
  new (&ev1) event_flags { "ev1" };

  // Local static storage for the event flags object instance.
  static std::aligned_storage<sizeof(event_flags), alignof(event_flags)>::type ev2;

  // Use placement new, to explicitly call the constructor
  // and initialise the event flags.
  // Create a static event flags object instance.
  new (&ev2) event_flags { "ev2" };

  // ...

  // For completeness, call the event flags destructors, which for placement new
  // is no longer called automatically.
  ev1.~event_flags();
  ev2.~event_flags();

  return 0;
}
```

Instancias de objetos de Sinalizadores de eventos podem ser criados no stack local, por exemplo no stack da thread principal. Apenas tenha a certeza que o stack seja grande o suficiente para gravar todos os objetos 

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a local event flags object instance.
  event_flags ev1 { "ev1" };

  // Beware of local static instances, since they'll use atexit()
  // to register the destructor; avoid and prefer placement new, as before.
  // static event_flags ev2 { "ev2" };

  // ...

  // The local event flags are destroyed automatically before exiting this block.
  return 0;
}
```

Um exemplo similar, mas escrito em C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

int
os_main (int argc, char* argv[])
{
  // ...

  // Local storage for the event flags object instance.
  os_evflags_t ev1;

  // Create an event flags object instance.
  os_evflags_create(&ev1, "ev1", NULL);

  // ...

  // For completeness, destroy the event flags.
  os_evflags_destroy(&ev1);

  return 0;
}
```

Para um controle total sobre a criação dos sinalizadores de eventos (por exemplo para definir um clock personalizado), o mecanismos de atributos dos sinalizadores de eventos podem ser usados.

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

int
os_main (int argc, char* argv[])
{
  // ...

  event_flags::attribute attr;
  attr.clock = &rtclock;

  // Create an event flags object instance.
  event_flags ev { "ev", attr };

  // ...

  // The local event flags are destroyed automatically before exiting this block.
  return 0;
}
```

Um exemplo similar, mas escrito em C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

int
os_main (int argc, char* argv[])
{
  // ...

  os_evflags_attr_t attr;
  os_evflags_attr_init(&attr);

  attr.clock = os_clock_get_rtclock();

  // Local storage for the event flags object instance.
  os_evflags_t ev;

  // Create an event flags object instance.
  os_evflags_create(&ev, "ev", &attr);

  // ...

  // For completeness, destroy the event flags.
  os_evflags_destroy(&ev);

  return 0;
}
```

O programador da aplicação pode criar um número ilimitado de grupos de sinalizadores de eventos (limitados apenas pela RAM disponíveis).

## Lançando sinalizadores de Eventos

Em termos da linguagem de programação, lançar um sinalizador equivale a executar uma operação OR sobre os bits correspondentes na mascara de sinalizadores de eventos. Uma vez lançado, os flags restantes lançados até eles serem verificados, ou explicitamente liberados. Lançando um sinalizador já lançado é uma operação _nop_.

Quando uma _thread_ ou uma _ISR_ lança um sinalizador, todas as _threads_ que tem sua condição de espera satisfeita será resumida.

### Lançando sinalizadores de de eventos das _ISRs_

É perfeitamente possível lançar sinalizadores de eventos das _ISRs_, geralmente para sinalizar _threads_ que aguardam que eventos ocorram em uma interrupção.

## Aguarde por sinalizadores de eventos

Uma _thread_ pode verificar a qualquer momento se uma conjunto de sinalizzadores esperados foi lançado é possível verificar se **todas** os sinalizadores no conjunto foram lançados, ou se **algum** sinalizador no conjunto foi lançado.

Cabe a aplicação determinar o que cada bit na mascara dos sinalizadores de evento significam e é possível usar para quantos sinalizaodres de evento forem preciso.


``` c++
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

event_flags ev { "ev" };

// Thread function.
void*
th_func(void* args)
{
  // Wait for two event flags.
  result_t res;
  res = ev.timed_wait(0x3, 100);
  if (res == os_ok)
    {
      trace::printf("Both flags raised\n");
    }
  else if (res == ETIMEDOUT)
    {
      trace::printf("Timeout\n");
    }

  return nullptr;
}

int
os_main (int argc, char* argv[])
{
  // ...

  // Create the thread. Stack is dynamically allocated.
  thread th { "th", th_func, nullptr };

  // Raise one flag. The condition is not enough to resume the thread.
  ev.raise(0x1);

  // Pretend the thread has something important to do.
  sysclock.sleep_for(10);

  // Raise second flag. The thread will be resumed.
  ev.raise(0x2);

  // Wait for the thread to terminate.
  th.join();

  // ...
  return 0;
}
```

Um exemplo similar, porém escrito em C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

// Global static storage for the event flags object instance.
os_evflags_t ev;

// Thread function.
void*
th_func(void* args)
{
  // Wait for two event flags.
  // In C there are no defaults, all parameters must be specified.
  os_result_t res;
  res = os_evflags_timed_wait(&ev, 0x3, 100, NULL, os_flags_mode_all | os_flags_mode_clear);
  if (res == os_ok)
    {
      trace_printf("Both flags raised\n");
    }
  else if (res == ETIMEDOUT)
    {
      trace_printf("Timeout\n");
    }

  return NULL;
}

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a global event flags object instance.
  os_evflags_create(&ev, "ev", NULL);

  // Local storage for the thread object.
  os_thread_t th;

  // Initialise the thread object and allocate the thread stack.
  os_thread_create(&th, "th", th_func, NULL, NULL);

  // Raise one flag. The condition is not enough to resume the thread.
  os_evflags_raise(&ev, 0x1, NULL);

  // Pretend the thread has something important to do.
  os_sysclock_sleep_for(10);

  // Raise second flag. The thread will be resumed.
  os_evflags_raise(&ev, 0x2, NULL);

  // Wait for the thread to terminate.
  os_thread_join(&th, NULL);

  // ...

  // For completeness, destroy the thread.
  os_thread_destroy(&th);

  // For completeness, destroy the event flags.
  os_evflags_destroy(&ev);

  return 0;
}
```

Para verificar se algum sinalizador no conjunto foi lançado, use `flags::mode::any` (em C use `os_flags_mode_any`).

## Multiplas threads aguardando nos sinalizadores de eventos

É possível para várias _threads_ aguardar nos mesmos objetos de sinalização de eventos (independete se os bits sejam os mesmos ou sejam diferentes), cada _thread_ com seu próprio _timeout_.

Quando uma _thread_ ou uma _ISR_ lança um sinalizador em um objeto de sinalização de eventos, todas as threas que tem sua condição de espera satisfeita será continuadas.

## Outras funções de sinalizadores de eventos

Como apresentado no exemplo acima, o caso de uso comum dos sinalizadores de eventos é automaticamente limpar os sinalizadores após testa-los. Para casos especiais pode ser útil ler e limpar cada sinalizador individualmente.

### Obtendo o nome dos sinalizadores de evento

Os sinalizadores de eventos são strings opcionais definidas durante a criação da instância do objeto de sinalização de eventos. Ele é geralmente usado para identificar um sinalizador de eventos durante a seção de depuração.

A API C++ é:

``` c++
event_flags ev { "ev" };

const char* name = ev.name();
```

A API C é:

``` c
os_evflags_t ev;
os_evflags_create(&ev, "ev" };

const char* name = os_evflags_get_name(&ev);
```

### Obtendo sinalizadores individuais

É possível ler de forma seletiva os sinalizadores de eventos, e possivelmente limpa-los depois. para evitar a limpeza, informe o modo 0.

Somente o sinalizador presente na mascara serão afetados.

``` c++
flags::mask_t mask = ev.get(0x2, flags::mode::clear);
```

Um exemplo similar, mas escrito em C:

``` c
os_flags_mask_t mask = os_evflags_get(&ev, 0x2, os_flags_mode_clear);
```

### Limpando sinalizadores individualmente

É possível seletivamente limpar um sinalizador de eventos, e possívelmente pegar o valor de outros sinalizadores antes de limpa-lo. Se o ponteiro informado for _null_, o valor anterior do sinalizador informado será perdido.

Somente o sinalizador presente na mascara são afetados.

``` c++
flags::mask_t mask;
ev.clear(0x2, &mask);
```

Um exemplo similar, mas escrito em C:

``` c
os_flags_mask_t mask;
os_evflags_clear(&ev, 0x2, &mask);
```

## Destrindo sinalizadores de eventos

No C++, se um sinalizador de eventos for criado usando a forma normal, o destrutor são automáticamente invocados quando o bloco corrente sair, ou, para a instância global de sinalizador de eventos,, após a função `main()` retornar. flags de eventos  criados com o operador `new` precisam ser destruidos manualmente.

No C, todos os sinalizadores de eventos devem ser destruidos por chamadas explicitas para `os_evflags_destroy()`.

Não deverá haver _threads_ aguardando o sinalizador de eventos quando a instancia do objeto for destruida, caso crontário uma asserção será disparada.
