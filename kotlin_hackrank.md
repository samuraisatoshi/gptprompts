1. **Perguntas práticas e conceituais sobre Ciclo de Vida de Activity**
2. **Perguntas práticas e conceituais sobre Ciclo de Vida de Service**
3. **Perguntas práticas e conceituais sobre Broadcast Receivers**
4. **Trabalhar com dados em background**
5. **Multi-threading e Coroutines**
6. **DataBinding e MVVM**
7. **Hash Tables**
8. **Conceitos de MVVM**

### Avaliação Técnica para 90 minutos dividida por tópicos:

### 1. Ciclo de Vida de Activity

**Pergunta 1 (Conceitual):**

**Descrição:** Descreva brevemente o ciclo de vida de uma Activity no Android e explique as diferenças entre os métodos `onCreate()`, `onStart()`, `onResume()`, `onPause()`, `onStop()` e `onDestroy()`.

**Tempo:** 10 minutos

**Resposta Esperada:**
```plaintext
O ciclo de vida de uma Activity no Android consiste em uma série de estados pelos quais a Activity passa desde sua criação até a sua destruição. Os métodos do ciclo de vida incluem:

- onCreate(): Chamado quando a Activity é criada. É aqui que você inicializa a Activity, configurando o layout com setContentView e inicializando dados necessários.
- onStart(): Chamado quando a Activity está prestes a se tornar visível para o usuário.
- onResume(): Chamado quando a Activity começa a interagir com o usuário. A Activity está no topo da pilha e em modo de execução.
- onPause(): Chamado quando a Activity é parcialmente obscurecida por outra Activity. Não recebe mais entrada do usuário, mas ainda está visível.
- onStop(): Chamado quando a Activity não está mais visível para o usuário. Pode ser seguido de onRestart() se a Activity estiver sendo recriada.
- onDestroy(): Chamado antes da Activity ser destruída, seja pelo sistema (para liberar memória) ou pelo próprio código.
```

### 2. Ciclo de Vida de Service

**Pergunta 2 (Conceitual):**

**Descrição:** Explique o ciclo de vida de um Service no Android e diferencie entre um service iniciado (Started Service) e um service vinculado (Bound Service).

**Tempo:** 10 minutos

**Resposta Esperada:**
```plaintext
O ciclo de vida de um Service no Android pode ser dividido entre Started Service e Bound Service.

- Started Service: É iniciado quando o componente (como uma Activity) chama startService(). O Service entra no estado `START_STICKY`, continua rodando em segundo plano mesmo após a ligação com o componente ser encerrada. Os métodos principais são:
  - onCreate(): Chamado quando o Service é criado pela primeira vez.
  - onStartCommand(): Chamado quando um componente solicita um serviço ao chamar startService(). Aqui, você implementa a lógica de longa duração.
  - onDestroy(): Chamado quando o Service está sendo finalizado.

- Bound Service: É ligado a um componente (como uma Activity) através do método bindService(). Ele permite que um componente se ligue ao Service e interaja diretamente, pegando dados ou chamando métodos. Os métodos principais são:
  - onBind(): Chamado quando o serviço é vinculado por um cliente. Retorna um IBinder que o cliente usa para interagir com o serviço.
  - onUnbind(): Chamado quando todos os clientes se desanexam de um interface de serviço específico previamente ativado.
  - onRebind(): Chamado quando novos clientes são ligados ao serviço após terem sido desligados anteriormente.
  - onDestroy(): Chamado quando o Service está sendo finalizado.
```

### 3. Broadcast Receivers

**Pergunta 3 (Conceitual):**

**Descrição:** O que são Broadcast Receivers no Android? Explique como eles funcionam e forneça um exemplo de caso de uso.

**Tempo:** 10 minutos

**Resposta Esperada:**
```plaintext
Broadcast Receivers são componentes que permitem que uma aplicação seja informada sobre eventos do sistema ou aplicacionais. Funcionam ouvindo "broadcasts" (intentos genéricos enviados por componentes do sistema ou aplicacionais).

Exemplo de caso de uso:
- Reagindo à conectividade de rede: Um BroadcastReceiver pode responder a mudanças na conectividade de rede, ajustando o comportamento da aplicação quando a internet está disponível ou não.

Exemplo básico de Broadcast Receiver:
```kotlin
class NetworkChangeReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        if ("android.net.conn.CONNECTIVITY_CHANGE" == intent?.action) {
            // Lógica para tratar mudanças na conectividade
        }
    }
}

// Registro no AndroidManifest.xml
<receiver android:name=".NetworkChangeReceiver">
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
</receiver>
```
```

### 4. Trabalhar com dados em background

**Pergunta 4 (Conceitual):**

**Descrição:** Quais são as principais formas de realizar operações em background no Android? Compare e contraste `Thread`, `AsyncTask`, `WorkManager` e `Coroutines`.

**Tempo:** 10 minutos

**Resposta Esperada:**
```plaintext
As principais formas de realizar operações em background no Android incluem:

1. Thread:
   - Tradicional, mas pode ser difícil de gerenciar, especialmente com várias threads e manipulação de UI.
   
2. AsyncTask:
   - Classe mais antiga para tarefas curtas, que oferece um método simples de encadear threads ao código de UI.
   - Desencorajado para uso em tarefas longas devido a problemas de gerenciamento de ciclo de vida e eficiência.
   
3. WorkManager:
   - API compatível para tarefas de longo prazo e recorrentes que precisam garantir execução até completar.
   - Excelente para tarefas robustas que precisam de execução mesmo após a aplicação ser encerrada.

4. Coroutines (Kotlin):
   - Nova abordagem simplificada para multitarefa, ideal para operações assíncronas.
   - Facilita a leitura de código sequencial e é fácil de cancelar e gerenciar com `CoroutineScope`.

**Conclusão:**
Cada um tem seu próprio caso de uso. `WorkManager` é recomendado para tarefas garantidas e recorrentes, enquanto `Coroutines` são ideais para tarefas assíncronas e simples.
```

### 5. Multi-threading e Coroutines

**Pergunta / Desafio 5:**

**Descrição:** Implemente um sistema que gerencie múltiplas coroutines para simular downloads concorrentes de arquivos. 

**Tempo:** 10 minutos

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

### 6. DataBinding e MVVM

**Pergunta / Desafio 6:**

**Descrição:** Implemente um exemplo básico de MVVM utilizando DataBinding no Android.

**Tempo:** 10 minutos

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

### 7. Hash Tables

**Pergunta / Desafio 7:**

**Descrição:** Implemente uma tabela de hash com as operações básicas: inserção (`put`), busca (`get`) e remoção (`remove`).

**Tempo:** 10 minutos

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

### 8. Conceitos de MVVM

**Pergunta 8 (Conceitual):**

**Descrição:** Explique brevemente os principais conceitos do padrão arquitetônico MVVM e como ele se diferencia de outros padrões como MVP e MVC na construção de aplicações Android.

**Tempo:** 5 minutos

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

- Ciclo de Vida de Activity: 10 minutos
- Ciclo de Vida de Service: 10 minutos
- Broadcast Receivers: 10 minutos
- Trabalhar com dados em background: 10 minutos
- Multi-threading e Coroutines: 10 minutos
- DataBinding e MVVM: 10 minutos
- Hash Tables: 10 minutos
- Conceitos de MVVM: 5 minutos

**Tempo Total:** 85 minutos

### Implementação no HackerRank

Cada uma das perguntas pode ser configurada no HackerRank conforme descrito, ajustando as descrições e esqueleto de código para os desafios práticos e fornecendo as expectativas de respostas para perguntas conceituais. Certifique-se de definir o tempo permitido para cada pergunta conforme estimado acima para garantir que o teste completo se encaixe dentro dos 90 minutos disponíveis. 

Desta forma, você cobre uma ampla gama de conhecimentos e habilidades relacionadas ao desenvolvimento Android com Kotlin dentro do limite de tempo disponível.
