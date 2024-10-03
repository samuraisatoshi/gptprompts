### Avaliação Técnica para 90 minutos:

### Seção 1: Multi-threading, Coroutines e Channels

**Pergunta / Desafio 1:**

**Descrição:** Implemente um sistema que gerencie múltiplas coroutines para simular downloads concorrentes de arquivos. Cada download deve retornar um valor específico para mostrar que a coroutine foi executada corretamente.

**Tempo:** 20 minutos

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

### Seção 2: DataBinding e MVVM

**Pergunta / Desafio 2:**

**Descrição:** Implemente um exemplo básico de MVVM utilizando DataBinding no Android. O exemplo deve ligar dados simples de um `ViewModel` para uma `TextView` em um layout.

**Tempo:** 25 minutos

**Código de Esqueleto:**
```kotlin
// ViewModel.kt
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class SimpleViewModel : ViewModel() {
    private val _text = MutableLiveData("Hello, MVVM with DataBinding!")
    val text: LiveData<String> get() = _text
}

// activity_main.xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable
            name="viewModel"
            type="com.example.app.SimpleViewModel" />
    </data>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <TextView
            android:id="@+id/textView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{viewModel.text}" />
    </LinearLayout>
</layout>

// MainActivity.kt
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.databinding.DataBindingUtil
import com.example.app.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private val viewModel: SimpleViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding: ActivityMainBinding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        binding.viewModel = viewModel
        binding.lifecycleOwner = this
    }
}
```

### Seção 3: Hash Tables

**Pergunta / Desafio 3:**

**Descrição:** Implemente uma tabela de hash com as operações básicas: inserção (`put`), busca (`get`) e remoção (`remove`).

**Tempo:** 20 minutos

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

### Seção 4: Conceitos de MVVM

**Pergunta 4:**

**Descrição:** Explique brevemente os principais conceitos do padrão arquitetônico MVVM e como ele se diferencia de outros padrões como MVP e MVC na construção de aplicações Android.

**Tempo:** 15 minutos

**Resposta Esperada:**
```plaintext
MVVM (Model-View-ViewModel) é um padrão arquitetônico em que:
- Model: Representa a camada de dados ou lógica de negócios.
- View: Representa a interface de usuário e exibição dos dados.
- ViewModel: Atua como um intermediário entre Model e View, mantendo o estado e fornecendo dados que a View consome, geralmente via DataBinding.

Diferenças entre MVP e MVC:
- MVP (Model-View-Presenter): O Presenter atua como intermediário, com a View sendo passiva. É adequado para separar regras de apresentação.
- MVC (Model-View-Controller): O Controller atua como intermediário com a View sendo atualizada diretamente pelo Controller. É comum em frameworks web, mas pode criar ligações mais rígidas.
Em MVVM, o ViewModel pode ser reutilizável e testável sem dependências da View, ao utilizar DataBinding para comunicação entre View e ViewModel.
```

### Resumo do Tempo:

- Multi-threading, Coroutines e Channels: 20 minutos
- DataBinding e MVVM: 25 minutos
- Hash Tables: 20 minutos
- Conceitos de MVVM: 15 minutos

**Tempo Total:** 80 minutos

### Implementação no HackerRank

Você pode adicionar estas perguntas no HackerRank, garantindo que cada pergunta seja clara e estruturada para permitir avaliação automática quando possível. Aqui está um exemplo de como configurar uma questão no HackerRank:

### Configuração de Questão no HackerRank:

**Título:** Gestão de Coroutines para Downloads Concorrentes

**Descrição:**
```plaintext
Implemente um sistema que gerencie múltiplas coroutines para simular downloads concorrentes de arquivos. Cada download deve retornar um valor específico para mostrar que a coroutine foi executada corretamente. Complete a função `downloadFile` e crie a estrutura para gerenciar múltiplos downloads concorrentes. Use Kotlin e a biblioteca `kotlinx.coroutines`.
```

**Tempo:** 20 minutos

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

### Configuração para outras seções:

Repita o processo de configuração para as seções de DataBinding e MVVM, Hash Tables e Conceitos de MVVM, ajustando o tempo e as descrições conforme necessário.

Desta forma, você garante que importantes áreas de conhecimento sejam avaliadas dentro do limite de tempo disponível.
