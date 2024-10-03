
### Avaliação Técnica Compacta para 90 minutos:

### Seção 1: Multi-threading, Coroutines e Channels

**Pergunta / Desafio 1:**

**Descrição:** Implemente um sistema que gerencie múltiplas coroutines para simular downloads concorrentes de arquivos. Cada download deve retornar um valor específico para mostrar que a coroutine foi executada corretamente.

**Tempo:** 25 minutos

**Código de Esqueleto:**
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.channels.Channel

suspend fun downloadFile(index: Int): String {
    // Simular o tempo de download
    delay(1000L * index)
    return "File $index downloaded"
}

fun main() = runBlocking {
    val numberOfDownloads = 3
    val channel = Channel<String>()
    
    for (i in 1..numberOfDownloads) {
        launch {
            val result = downloadFile(i)
            channel.send(result)
        }
    }
    
    repeat(numberOfDownloads) {
        println(channel.receive())
    }
}
```

### Seção 2: Hash Tables

**Pergunta / Desafio 2:**

**Descrição:** Implemente uma tabela de hash com as operações básicas: inserção (`put`), busca (`get`) e remoção (`remove`).

**Tempo:** 25 minutos

**Código de Esqueleto:**
```kotlin
class HashTable<K, V>(private val initialCapacity: Int = 16) {

    data class Entry<K, V>(val key: K, val value: V)

    private val buckets = Array<MutableList<Entry<K, V>>>(initialCapacity) { mutableListOf() }

    fun put(key: K, value: V) {
        val bucketIndex = key.hashCode() % buckets.size
        val bucket = buckets[bucketIndex]
        val existingEntry = bucket.find { it.key == key }
        if (existingEntry != null) {
            bucket.remove(existingEntry)
        }
        bucket.add(Entry(key, value))
    }

    fun get(key: K): V? {
        val bucketIndex = key.hashCode() % buckets.size
        val bucket = buckets[bucketIndex]
        return bucket.find { it.key == key }?.value
    }

    fun remove(key: K): Boolean {
        val bucketIndex = key.hashCode() % buckets.size
        val bucket = buckets[bucketIndex]
        val existingEntry = bucket.find { it.key == key }
        return if (existingEntry != null) {
            bucket.remove(existingEntry)
            true
        } else {
            false
        }
    }
}

fun main() {
    val hashTable = HashTable<String, Int>()
    hashTable.put("One", 1)
    println(hashTable.get("One"))  // Expected: 1
    hashTable.remove("One")
    println(hashTable.get("One"))  // Expected: null
}
```

### Seção 3: SOLID

**Pergunta / Desafio 3:**

**Descrição:** Desenvolva um módulo de pagamento que siga os princípios SOLID. Utilize o princípio de responsabilidade única e a inversão de dependência.

**Tempo:** 20 minutos

**Código de Esqueleto:**
```kotlin
interface PaymentProcessor {
    fun processPayment(amount: Double)
}

class CreditCardProcessor : PaymentProcessor {
    override fun processPayment(amount: Double) {
        // Implementação do processamento de pagamento com cartão de crédito
        println("Processing credit card payment of $$amount")
    }
}

class PaymentService(private val processor: PaymentProcessor) {
    fun makePayment(amount: Double) {
        processor.processPayment(amount)
    }
}

fun main() {
    val paymentService = PaymentService(CreditCardProcessor())
    paymentService.makePayment(100.0)  // Expected: Processing credit card payment of $100.0
}
```

### Seção 4: Programação Orientada a Eventos

**Pergunta / Desafio 4:**

**Descrição:** Utilize Kotlin Flow para criar um fluxo que emite valores a cada segundo e permita que um consumidor processe esses valores à medida que são emitidos.

**Tempo:** 20 minutos

**Código de Esqueleto:**
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

fun simpleFlow(): Flow<Int> = flow {
    for (i in 1..3) {
        delay(1000L)
        emit(i)
    }
}

fun main() = runBlocking<Unit> {
    simpleFlow().collect { value ->
        println(value)  // Expected: 1, 2, 3 (one per second)
    }
}
```

### Resumo do Tempo:

- Multi-threading, Coroutines e Channels: 25 minutos
- Hash Tables: 25 minutos
- SOLID: 20 minutos
- Programação Orientada a Eventos: 20 minutos

**Tempo Total:** 90 minutos

### Implementação no HackerRank

Ao configurar cada questão no HackerRank, adicione as descrições detalhadas e esqueleto de código fornecidos acima. Defina o tempo permitido para cada questão de acordo com as estimativas de tempo para garantir que o desafio completo não exceda 90 minutos.

### Configuração de Questão no HackerRank:

**Título:** Gestão de Coroutines para Downloads Concorrentes

**Descrição:**
```plaintext
Implemente um sistema que gerencie múltiplas coroutines para simular downloads concorrentes de arquivos. Cada download deve retornar um valor específico para mostrar que a coroutine foi executada corretamente. Complete a função `downloadFile` e crie a estrutura para gerenciar múltiplos downloads concorrentes. Use Kotlin e a biblioteca `kotlinx.coroutines`.
```

**Duração:** 25 minutos

**Esqueleto do Código:**
```kotlin
import kotlinx.coroutines.*
import kotlinx.coroutines.channels.Channel

suspend fun downloadFile(index: Int): String {
    // Simular o tempo de download
    delay(1000L * index)
    return "File $index downloaded"
}

fun main() = runBlocking {
    val numberOfDownloads = 3
    val channel = Channel<String>()
    
    for (i in 1..numberOfDownloads) {
        launch {
            val result = downloadFile(i)
            channel.send(result)
        }
    }
    
    repeat(numberOfDownloads) {
        println(channel.receive())
    }
}
```

### Repetir para cada seção:

**Seção 2:**
**Título:** Implementação de Tabela de Hash
**Duração:** 25 minutos

**Seção 3:**
**Título:** Módulo de Pagamento seguindo os Princípios SOLID
**Duração:** 20 minutos

**Seção 4:**
**Título:** Programação Orientada a Eventos com Kotlin Flow
**Duração:** 20 minutos

Certifique-se de que cada pergunta esteja claramente definida e testável na plataforma HackerRank. Com esse conjunto de questões, você obterá uma avaliação eficaz das habilidades dos desenvolvedores dentro do limite de tempo disponível.
