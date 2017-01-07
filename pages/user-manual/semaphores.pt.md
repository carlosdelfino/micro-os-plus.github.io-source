---
layout: page
lang: pt
permalink: /pt/user-manual/semaphores/
title: Semáforos
author: Liviu Ionescu
translator: Carlos Delfino

date: 2016-07-12 10:36:00 +0300
last_updated_at:  2016-08-26 17:30:00 +0300

---
{% comment %}

start_translate_at:  2016-08-24 20:30:00 +0300

Base Commit:
 - aac11b8d05198ec0a390c2c046e9578e92726ad0
 - 0ff10d71be7c5551398ddf85685efb53fa6a37e7

{% endcomment %}

## Visão Geral

Semáforos são um dos mais antigos mecanismos introduzidos pelos sistemas multi tarefas, sendo usado tanto para gerenciar recursos comuns como sincronização.

Gerenciando recursos comuns, no seu formato mais simples, evita que varias _threads_ concorram pelo uso de um recurso compartilhado, bloqueando o acesso de todas as outras _threads_ até a _thread_ que adquiriu o recuso o libere.

A Sincronização é geralmente requerida para implementar de forma eficientemente o bloqueio de I/O; quando uma _thread_ requer algum dado que não está ainda disponível (por exemplo para executar um `read()`), não é eficiente ficar sondando até que os dados se tornem disponíveis, mas é muito melhor suspender a _thread_ e preparar para que o produtor de dados (normalmente um ISR) retome a _thread_  quando o dado está disponível.

No µOS++ há dois mecanismos básicos de sincronização: **semáforos** e **_flags_ de eventos**.

Um [semáforo](https://pt.wikipedia.org/wiki/Sem%C3%A1foro_(computa%C3%A7%C3%A3o)) é um mecanismo de sincronização oferecido pela maioria dos sistemas multitarefas. Em sua forma mais simples, um semáforo é similar a um semáforo de transito no mundo real, que bloqueia o acesso a um seguimento da rodovia em certas condições.

O conceito de semáforo foi introduzido em 1965 pelo cientista da computação holandês  [Edsger Dijkstra](https://pt.wikipedia.org/wiki/Edsger_Dijkstra) e historicamente ele diz ter sido inspirado pelo sistema de semáforo das ferrovias (o semáforo binário, que controla acesso para um simples recurso pela separação da seção critica através de primitivas `P(S)` e `V(S)`).

<div style="float:right; margin-left: 10px;">
<img src="{{ site.baseurl }}/assets/images/2016/160px-Rail-semaphore-signal-Dave-F.jpg" />
</div>

O conceito foi depois estendido por outro Holandês, Carel S. Scholten, para controlar acesso para um número arbitrário de recursos. Nesta proposta o semáforo pode ter um valor inicial (ou contar) maior que um (um semáforo de contagem).

Semáforos foram originalmente usados para controlar acesso a recursos compartilhados. Porém, dependendo da aplicação, o melhor mecanismo existe agora para gerenciar recursos compartilhados, como travas, mutexes, etc. Semáforos são melhores usados para sincronizar uma _thread_ com uma ISR ou outra _thread_ (ponto de encontro unilateral).

## Tipos de Semáforos

Há dois tipos de semáforos: **binários** e **contagem**

Atenção: no µOS++, mesmo que semáforos binários ou de contagens sejam definidos por classes diferentes, o objeto criado são atualmente os mesmos, mas construídos com parâmetros diferentes; semáforos binários são de fato semáforos contadores com o valor máximo definido para 1.

### Semáforos binários 

Já que nós mencionamos a analogia com um sistema ferroviário, permita imaginarmos ter uma pequena estação de trem, com uma simples plataforma. O primeiro trem a chegar na estação sem alguma restrição e parar na plataforma. Para evitar um segundo trem de entrar na estação e bater no primeiro, um semáforo é instalado a certa distância antes da estação. O semáforo da ferrovia tem uma mão vermelha que pode, ou estar elevado ou baixado. Semáforos modernos são elétricos, e também tem luzes (vermelho e verde). Ao primeiro trem entrar na estação, a mão é baixada e a luz se torna vermelha. Se um segundo trem chega, ele lé isto como "parar" e esperar. Quando o primeiro trem deixa a estação, o semáforo se alterna a luz se torna verde e o segundo trem pode entrar na estação.

Como um semáforo de ferrovia que tem dois estados, um semáforo binário tem somente dois valores, 0 ou 1. Se o valor é 0, o recurso associado com o semáforo não está disponível, e qualquer um que precisar dele, tem que esperar, como trem que parou no semáforo vermelho. quando o recurso se torna disponível, o semáforo é "informado" (posted), permitindo o segundo trem aguardando pelo semáforo para continuar.

Dependendo do uso do semáforo, ele pode iniciar ou com 0 (quando usado para sincronização) ou com 1 (quando usado para proteger um simples recursos compartilhado).

O que um semáforo binário tem a mais que um semáforo de ferrovia, é um método de sinalização (pense neste mecanismo como uma corneta alta usada para acordar o maquinista que dorme, aguardando o semáforo).

### Semáforo de Contagem

Para continuar a analogia com o sistema ferroviário, e se nos temos uma grande estação de trem, com múltiplas plataformas, onde muitos trens podem estar presente ao mesmo tempo? Bem, a solução é similar, mas a lógica do semáforo é manter o controle sobre o número de trens na estação, e acender a luz vermelha quando todas os trilhos estiverem ocupados. Quando um trem deixa a estação, o semáforo pode se tornar verde e se há um trem aguardando, ele será permitido entrar na estação.

Um semáforo de contagem tem um contador com um limite, representando o número máximo de recursos disponíveis.

Dependendo do uso do semáforo, ele usualmente inicia ou com 0 (quando usado para sincronização) ou em seu limite (quando usado para proteger múltiplos recursos compartilhados).

Assumindo que ele inicia em zero, com nenhum recurso disponível, o semáforo é "informado" (posted) cada vez um novo recurso se torna disponível, que incrementa o contador. Quando o máximo é atingido, "informes" (posts) futuras falharão e o contador se mantem inalterado.

No outro lado, quando recursos precisam ser consumidos, e quanto o contador é positivo, a _thread_ requerente será permitida acessar o recurso e cada requisição, o contador será decrementado.

Quando o contador atinge 0, nenhum recurso está mais disponível e a _thread_ requerente é suspendida, até o semáforo ser informado.

Um semáforo de contagem é usado quando elementos de um recurso pode ser usados por mais que uma _thread_ ao mesmo tempo. Por exemplo, um semáforo de contagem pode ser usado no gerenciamento da área do buffer.


<div style="text-align:center">
<img src="{{ site.baseurl }}/assets/images/2016/semaphore.png" />
<p>Serviço de semáforo</p>
</div>

## Criando semáforos

Por questões de convenção, µOS++ tem vários funções pra criação de semáforos. Semáforos pode ser criados como objetos locais no na pilha da função, ou como objetos globais, semáforos pode ser binário, ou de contagem, semáforos podem ser criados com características padrões ou com atributos personalizados, e assim por diante.

Quando usado para sincronizar _threads_ com ISRs, a forma mais simples para acessar semáforo é quando ele são criados como objetos globais.

O valor inicial para o semáforo é tipicamente zero (0), indicando que o evento não tenha ainda ocorrido, ou não há recursos. É possível inicializar o semáforo com um valor diferente de zero, indicando que o semáforo inicialmente contém o número de recursos.

Em C++, os semáforos globais são criados e inicializados pelo mecanismo de construtor estático global.

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Create a global binary semaphore object instance,
// with the initial count as 0.
semaphore_binary sb1 { "sb1", 0 };

// Create a global binary semaphore object instance,
// Create a global binary semaphore,
// with the initial count as 1.
semaphore_binary sb2 { "sb2", 1 };

// Create a global binary semaphore object instance,
// with max 7 items and the initial count as 0.
semaphore_counting sc1 { "sc1", 7, 0 };

// Create a global binary semaphore object instance,
// with max 7 items and the initial count as 7.
semaphore_counting sc2 { "sc2", 7, 7 };

int
os_main (int argc, char* argv[])
{
  // ...

  // Not much to do, the semaphores were created by the static
  // constructors, before entering main().

  // ...

  return 0;
}

// All global semaphores are automatically destroyed if os_main() returns.

```

Um exemplo similar, mas escrito em C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

// Global static storage for the semaphore objects instances.
os_semaphore_t sb1;
os_semaphore_t sb2;
os_semaphore_t sc1;
os_semaphore_t sc2;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a global binary semaphore object instance,
  // with the initial count as 0.
  os_semaphore_binary_create(&sb1, "sb1", 0);

  // Create a global binary semaphore object instance,
  // with the initial count as 1.
  os_semaphore_binary_create(&sb2, "sb2", 1);

  // Create a global binary semaphore object instance,
  // with max 7 items and the initial count as 0.
  os_semaphore_counting_create(&sc1, "sc1", 7, 0);

  // Create a global binary semaphore object instance,
  // with max 7 items and the initial count as 7.
  os_semaphore_counting_create(&sc2, "sc2", 7, 7);

  // ...

  // For completeness, destroy the semaphores.
  os_semaphore_destroy(&sb1);
  os_semaphore_destroy(&sb2);
  os_semaphore_destroy(&sc1);
  os_semaphore_destroy(&sc2);

  return 0;
}
```

Em C++, se ele é necessário controlar o momento quando as instâncias de objetos globais são criados, é possível separadamente alocar o armazenamento de variáveis globais, então usa a declaração com o operador `new` para então inicializa-lo.

``` c++
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Global static storage for the semaphore object instance.
// This storage is set to 0 as any uninitialised variable.
std::aligned_storage<sizeof(semaphore_binary), alignof(semaphore_binary)>::type sb1;

int
os_main (int argc, char* argv[])
{
  // ...

  // Use placement new, to explicitly call the constructor
  // and initialise the semaphore.
  // Create a global binary semaphore object instance,
  // with the initial count as 1.
  new (&sb1) semaphore_binary { "sb1", 1 };

  // Local static storage for the semaphore object instance.
  static std::aligned_storage<sizeof(semaphore_counting), alignof(semaphore_counting)>::type sc1;

  // Use placement new, to explicitly call the constructor
  // and initialise the semaphore.
  // Create a static counting semaphore object instance,
  // max 7 items and the initial count as 0.
  new (&sc1) semaphore_counting { "sc1", 7, 0 };

  // ...

  // For completeness, call the semaphores destructors, which for placement new
  // is no longer called automatically.
  sb1.~semaphore_binary();
  sc1.~semaphore_counting();

  return 0;
}
```

Instâncias de objetos de semáforo podem também ser criadas na pilha local, por exemplo na pilha de _thread_ principal. Tenha certeza que a pilha é grande o suficiente para gravar todos os objetos locais definidos.

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a local binary semaphore object instance,
  // with the initial count as 0.
  semaphore_binary sb1 { "sb1", 0 };

  // Create a local binary semaphore object instance,
  // with the initial count as 1.
  semaphore_binary sb2 { "sb2", 1 };

  // Create a local binary semaphore object instance,
  // with max 7 items and the initial count as 0.
  semaphore_counting sc1 { "sc1", 7, 0 };

  // Create a local binary semaphore object instance,
  // with max 7 items and the initial count as 7.
  semaphore_counting sc2 { "sc2", 7, 7 };

  // Beware of local static instances, since they'll use atexit()
  // to register the destructor; avoid and prefer placement new, as before.
  // static semaphore_binary sb3 { "sb3" };

  // ...

  // The local semaphores are destroyed automatically before exiting this block.
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

  // Local storage for the semaphore object instance.
  os_semaphore_t sb1;

  // Create a binary semaphore,
  // with the initial count as 0.
  os_semaphore_binary_create(&sb1, "sb1", 0);

  // Local storage for the semaphore object instance.
  os_semaphore_t sb2;

  // Create a binary semaphore,
  // with the initial count as 1.
  os_semaphore_binary_create(&sb2, "sb2", 1);

  // Local storage for the semaphore object instance.
  os_semaphore_t sc1;

  // Create a counting semaphore,
  // with max 7 items and the initial count as 0.
  os_semaphore_counting_create(&sc1, "sc1", 7, 0);

  // Local storage for the semaphore object instance.
  os_semaphore_t sc2;

  // Create a counting semaphore,
  // with max 7 items and the initial count as 7.
  os_semaphore_counting_create(&sc2, "sc2", 7, 7);

  // ...

  // For completeness, destroy the semaphores.
  os_semaphore_destroy(&sb1);
  os_semaphore_destroy(&sb2);
  os_semaphore_destroy(&sc1);
  os_semaphore_destroy(&sc2);

  return 0;
}
```

Para um total controle sobre a criação de semáforo (por exemplo para definir um relógio personalizado), o mecanismo de atributo do semáforo pode ser usado.

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

int
os_main (int argc, char* argv[])
{
  // ...

  semaphore::attribute_binary attr_b1 { 0 };
  attr_b1.clock = &rtclock;

  // Create a local binary semaphore object instance,
  // the initial count as 0.
  semaphore sb1 { "sb1", attr_b1 };

  semaphore::attribute_counting attr_c2 { 7, 0 };
  attr_c2.clock = &rtclock;

  // Create a local binary semaphore object instance,
  // with max 7 items and the initial count as 0.
  semaphore sc1 { "sc1", attr_c2 };

  // Attributes for a generic counting semaphore, with max 7 items
  // and the initial count as 7.
  semaphore::attribute attr_g1;
  attr_g1.sm_max_value = 7;
  attr_g1.sm_initial_value = 7;
  attr_g1.clock = &rtclock;

  // Create a generic counting semaphore, fully defined by attributes.
  semaphore sg1 { "sg1", attr_g1 };

  // ...

  // The local semaphores and the attributes are destroyed automatically
  // before exiting this block.
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

  os_semaphore_attr_t attr_b1;
  os_semaphore_attr_binary_init(&attr_b1, 0);

  attr_b1.clock = os_clock_get_rtclock();

  // Local storage for the semaphore object instance.
  os_semaphore_t sb1;

  // Create a binary semaphore,
  // with the initial count as 0.
  os_semaphore_create(&sb1, "sb1", &attr_b1);

  os_semaphore_attr_t attr_c2;
  os_semaphore_attr_counting_init(&attr_c2, 7, 0);

  attr_c2.clock = os_clock_get_rtclock();

  // Local storage for the semaphore object instance.
  os_semaphore_t sc1;

  // Create a counting semaphore,
  // with max 7 items and the initial count as 0.
  os_semaphore_create(&sc1, "sc1", &attr_c2);

  os_semaphore_attr_t attr_g1;
  os_semaphore_attr_init(&attr_g1);

  attr_g1.sm_max_value = 7;
  attr_g1.sm_initial_value = 7;
  attr_g1.clock = os_clock_get_rtclock();

  // Local storage for the semaphore object instance.
  os_semaphore_t sg1;

  // Create a generic counting semaphore, with max 7 items
  // and the initial count as 7.
  os_semaphore_create(&sg1, "sg1", &attr_g1);

  // ...

  // For completeness, destroy the semaphores.
  os_semaphore_destroy(&sb1);
  os_semaphore_destroy(&sc1);
  os_semaphore_destroy(&sg1);

  return 0;
}
```

O programador da aplicação pode ser criado com um número de ilimitado de semáforos (limitado somente pela memória RAM disponível).

## Informando ao semáforo

Quando um recurso associado com o semáforo se torna disponível, o semáforo é notificado.

O nome **post** (Informe) vem do POSIX; outros nomes usados são **p**, **signal**, **release**.

``` cpp
result_t res;
res = sem.post();
if (res == result::ok)
  {
    // The semaphore was posted.
  }
else if (res == EGAIN)
  {
    // The maximum count value was exceeded.
  }
```

Um exemplo similar, mas escrito em C:

``` c
os_result_t res;
res = os_semaphore_post(&sem);
if (res == os_ok)
  {
    // The semaphore was posted.
  }
else if (res == EGAIN)
  {
    // The maximum count value was exceeded.
  }
```

Quando um semáforo é corretamente informado, o valor é incrementado e a _thread_ mais antiga de alta prioridade aguarda (se alguma) ser adicionada para a lista READY, permitindo adquirir o semáforo.

Se alguma das _threads_ aguardando, tem uma prioridade mais alta que a _thread_ em execução no momento, µOS++ rodará a _thread_ de maior prioridade tornando-a pronta para `post()`. A _thread_ corrente é suspendida até ele se tornar a _thread_ de maior prioridade que está pronta para executar.

### Informando (Posting) o semaforo de uma ISRs

É perfeitamente possível informar (post) um semáforo de uma ISR, geralmente para sincronizar _threads_ que aguardam  eventos ocorrerem em interrupções.

## Waiting on semaphores

To synchronise access to a resource, a thread must invoke the semaphore `wait()` function.

If the resource is available, in other words if the semaphore was posted and the count value is positive, the count is decremented and the call returns.

If the count value is zero, this means that no more resources are available, and the calling thread will be suspended. The thread will resume either when the semaphore is posted, a timeout occurs, or the thread is interrupted.

Along with the semaphore’s value, µOS++ also keeps track of the threads waiting for the semaphore’s availability (a double linked list, ordered by thread priorities).

``` cpp
result_t res;
res = sem.timed_wait(100); // The timeout is 100 clock ticks.
if (res == result::ok)
  {
    // The wait ended when the semaphore was posted.
  }
else if (res == ETIMEDOUT)
  {
    // The wait ended due to timeout.
  }
else if (res == EINTR)
  {
    // The wait ended due to an explicit interruption request.
  }
```

A similar example, but written in C:

``` c
os_result_t res;
res = os_semaphore_timed_wait(&sem, 100); // The timeout is 100 clock ticks.
if (res == os_ok)
  {
    // The wait ended when the semaphore was posted.
  }
else if (res == ETIMEDOUT)
  {
    // The wait ended due to timeout.
  }
else if (res == EINTR)
  {
    // The wait ended due to an explicit interruption request.
  }
```

The name **wait** comes from POSIX; other names used are **V**, **acquire**, **pend**.

## Multiple threads waiting on a semaphore

When semaphores are used to manage common resources, it is possible for several threads to wait on the same semaphore, each with its own timeout.

<div style="text-align:center">
<img src="{{ site.baseurl }}/assets/images/2016/semaphore-multi-thread.png" />
<p>Multiple threads waiting on a semaphore</p>
</div>

When the semaphore is posted, µOS++ makes the highest-priority thread waiting on the semaphore ready to run. If at this moment any of the waiting threads has a higher priority than the currently running thread, µOS++ will run all higher-priority threads and only then return to the current thread.

## Other semaphore functions

The µOS++ semaphore API basically implements the POSIX semaphores, with several extensions.

### Getting the semaphore name

The semaphore name is an optional string defined during the semaphore object instance creation. It is generally used to identify the semaphore during debugging sessions.

The C++ API is:

``` c++
semaphore_binary sem { "sem", 0 };

const char* name = sem.name();
```

The C API is:

``` c
os_semaphore_t sem;
os_semaphore_create(&sem, "sem", 0 };

const char* name = os_semaphore_get_name(&sem);
```

### Getting the semaphore value

The semaphore value is the instantaneous counter value, if positive, or 0 if the semaphore exhausted its resources and there are threads waiting for it.

The C++ API is:

``` c++
std::size_t value = sem.value();
```

The C API is:

``` c
size_t value = os_semaphore_get_value(&sem);
```

### Getting the semaphore maximum value

The semaphore maximum value is a constant value set at semaphore creation, representing the total number of resources associated with the semaphore. For binary semaphores, these functions return 1.

The C++ API is:

``` c++
std::size_t value = sem.max_value();
```

The C API is:

``` c
size_t value = os_semaphore_get_max_value(&sem);
```

### Getting the semaphore initial value

The semaphore initial value is a constant value set at semaphore creation, representing the number of initial resources associated with the semaphore. Applications using semaphores for synchronisation usually start with this value set to 0, while applications that use semaphores for resource management start with this value equal to the number of resources.

The C++ API is:

``` c++
std::size_t value = sem.initial_value();
```

The C API is:

``` c
size_t value = os_semaphore_get_initial_value(&sem);
```

### Reseting the semaphore

Real-world applications may include monitoring mechanisms to detect erroneous conditions. Recovering from such conditions may require to return the semaphore to stable state; µOS++ provides a function to return the semaphore to the initial state after construction.

The C++ API is:

``` c++
sem.reset();
```

The C API is:

``` c
os_semaphore_reset(&sem);
```

## Destroying semaphores

In C++, if the semaphores were created using the normal way, the destructors are automatically invoked when the current block exits, or, for the global semaphores instances, after the `main()` function returns. Semaphores created with placement `new` need to be destructed manually.

In C, all semaphores must be destructed by explicit calls to `os_semaphore_destroy()`.

There should be no threads waiting on the semaphore when it is destroyed, otherwise an assert check will trigger.

## Using semaphores for resource management

Originally semaphores were used to control access to shared resources, for example to I/O devices.

The classical example includes two or more threads writing messages to an I/O port. Without any control mechanism, the characters from one thread messages might get intermingled with characters from other thread messages.

The solution is to add a mechanism that allows one thread to gain exclusive access to the device and prevent other threads to write characters before the message is fully transferred. Such a critical section may use a binary semaphore, initialised to 1, and brace writing messages with `wait()` and `post()` calls.

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Create a binary semaphore, with the initial count as 1.
semaphore_binary sem { "sem", 1 };

void
write_message(const char* msg)
{
  // Wait until no other thread is using the device and lock access to it.
  sem.wait();

  // Write the message, one character at a time, without fear
  // that other threads will intervene.
  while (*msg)
    {
      write_char(*msg);
      ++msg;
    }

  // Unlock access to the device.
  sem.post();
}
```

A similar example, but written in C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

// Global static global storage for the semaphore object instance.
os_semaphore_t sem;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a binary semaphore, with the initial count as 1.
  os_semaphore_binary_create(&sem, "sem", 1);

  // ...

  os_semaphore_destroy(&sem);

  return 0;
}

void
write_message(const char* msg)
{
  // Wait until no other thread is using the device and lock access to it.
  os_semaphore_wait(&sem);

  // Write the message, one character at a time, without fear
  // that other threads will intervene.
  while (*msg)
    {
      write_char(*msg);
      ++msg;
    }

  // Unlock access to the device.
  os_semaphore_post(&sem);
}
```
**
It is mandatory for the binary semaphore to be initialised with 1 (i.e. device available) for the first `wait()` to not block.

## Unilateral rendezvous

It is common for a thread to wait for event generated by an ISR (or another thread). In this case, no data needs to be exchanged, only the fact that the ISR or the thread (on the left) has occurred is important. Using a semaphore for this type of synchronization is called a **unilateral rendezvous**.

<div style="text-align:center">
<img src="{{ site.baseurl }}/assets/images/2016/semaphore-unilateral-rendezvous.png" />
<p>Unilateral rendezvous</p>
</div>

A unilateral rendezvous is used when a thread initiates an I/O operation and waits (i.e., calls `wait()`) for the semaphore to be posted. Lower priority threads are executed. When the I/O operation is complete, an ISR (or another thread) signals the semaphore (i.e., calls `post()`), and the thread is resumed.

<div style="text-align:center">
<img src="{{ site.baseurl }}/assets/images/2016/semaphore-unilateral-rendezvous-timeline.png" />
<p>Unilateral rendezvous timeline</p>
</div>

An example code in C++ is:

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Create a binary semaphore, with the initial count as 0.
semaphore_binary sem { "sem", 0 };

int
os_main (int argc, char* argv[])
{
  // ...

  // Not much to do, the semaphore was created by the static
  // constructors, before entering main().

  return 0;
}

static volatile uint8_t device_byte;

uint8_t
read_byte(void)
{
  // Wait until the semaphore is posted.
  sem.wait();

  return device_byte;
}

void
device_interrupt_service_routine(void)
{
  // Store the device data in a static location.
  device_byte = read_device_register();

  // Inform the reader that new data is available.
  sem.post();
}
```

A similar example, but written in C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

// Global static global storage for the semaphore object instance.
os_semaphore_t sem;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a binary semaphore, with the initial count as 0.
  os_semaphore_binary_create(&sem, "sem", 0);

  // ...

  os_semaphore_destroy(&sem);

  return 0;
}

static volatile uint8_t device_byte;

uint8_t
read_byte(void)
{
  // Wait until the semaphore is posted.
  os_semaphore_wait(&sem);

  return device_byte;
}

void
device_interrupt_service_routine(void)
{
  // Store the device data in a static location.
  device_byte = read_device_register();

  // Inform the reader that new data is available.
  os_semaphore_post(&sem);
}
```

This example is for demonstrative purposes only. A real-world example would probably need to use a circular buffer to store multiple bytes, to avoid loosing data when multiple interrupts occur before the `read_byte()` is called.

## Semaphore pitfalls

Semaphores are great synchronisation objects, especially with events occurring on interrupts.

However semaphores are subject to several problems, which must be known and addressed during system design.

### Unbalanced wait()/post()

When using the semaphores for managing common resources, it is absolutely mandatory to begin the critical section with `wait()` and to end it with `post()`. Any possibility to leave the critical section in the middle of it (via `return`, `break`, `continue`, `goto`, etc) will result in a deadlock and should be avoided.

### Recursive deadlock

Recursive deadlock can occur if a thread tries to lock a semaphore it has already locked. This can typically occur in libraries or recursive functions.

A typical scenario is to have a functional program, like the one protecting the `write_message()` function and try to "improve" it to protect a series of messages:

``` cpp
/// @file app-main.cpp
#include <cmsis-plus/rtos/os.h>

using namespace os;
using namespace os::rtos;

// Create a binary semaphore, with the initial count as 1.
semaphore_binary sem { "sem", 1 };

void
write_message(const char* msg)
{
  // Wait until no other thread is using the device and lock access to it.
  sem.wait();

  // Write the message, one character at a time, without fear
  // that other threads will intervene.
  while (*msg)
    {
      write_char(*msg);
      ++msg;
    }

  // Unlock access to the device.
  sem.post();
}

// ...

void
write_many_messages(void)
{
  sem.wait();

  write_message("one");
  write_message("two");
  write_message("three");

  sem.post();
}
```

A similar example, but written in C:

``` c
/// @file app-main.c
#include <cmsis-plus/rtos/os-c-api.h>

// Global static global storage for the semaphore object instance.
os_semaphore_t sem;

int
os_main (int argc, char* argv[])
{
  // ...

  // Create a binary semaphore, with the initial count as 1.
  os_semaphore_binary_create(&sem, "sem", 1);

  // ...

  os_semaphore_destroy(&sem);

  return 0;
}

void
write_message(const char* msg)
{
  // Wait until no other thread is using the device and lock access to it.
  os_semaphore_wait(&sem);

  // Write the message, one character at a time, without fear
  // that other threads will intervene.
  while (*msg)
    {
      write_char(*msg);
      ++msg;
    }

  // Unlock access to the device.
  os_semaphore_post(&sem);
}

void
write_many_messages(void)
{
  os_semaphore_wait(&sem);

  write_message("one");
  write_message("two");
  write_message("three");

  os_semaphore_post(&sem);
}

```

Well... bad idea! Since the semaphore was created as binary, the first `wait()` in `write_many_messages()` will lock it, and the inner `wait()` in `write_message()` will find it locked and block forever.

For such scenarios, recursive mutexes are definitely more appropriate.

### Thread-termination deadlock

What if a thread that locked a semaphore is terminated? Since the semaphore does not keep track of its owner, most implementations are not able to detect this and all thread waiting (or may wait in the future) will never acquire the semaphore and deadlock. To partially address this, it is recommended to use the `timed_wait()`, which specifies a timeout.

### Priority inversion

Semaphores are subject to a serious problem in real-time systems called [priority inversion]({{ site.baseurl }}/user-manual/basic-concepts/#priority-inversion--priority-inheritance), where a high priority thread becomes delayed for an indefinite period by a low priority thread, preventing it to meet its deadlines. A very high profile case was the [NASA JPL’s Mars Pathfinder](https://en.wikipedia.org/wiki/Mars_Pathfinder) spacecraft (see [What really happened on Mars?](http://research.microsoft.com/en-us/um/people/mbj/Mars_Pathfinder/))

One of the best solutions to prevent priority inversion is to use mutexes with priority inheritance.

### Wrong initialisation

If using semaphores for mutual exclusion is problematic, using them for synchronisation is fine, and the the typical scenario is unilateral rendezvous. However, for this to work, the semaphore must be created with an initial count value of 0 (zero).
