# Coroutines
## Paralelismo (use async):
```kotlin
import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis

suspend fun fetchData(num: Int): String {
    delay(1000) // Simula uma chamada assíncrona demorada
    return "Dados $num"
}

fun main() = runBlocking {
    val startTime = System.currentTimeMillis()

    val deferredResults = mutableListOf<Deferred<String>>()
    for (i in 1..5) {
        val deferred = async { fetchData(i) }
        deferredResults.add(deferred)
    }

    val results = deferredResults.awaitAll()
    results.forEach { println(it) }

    val endTime = System.currentTimeMillis()
    val totalTime = endTime - startTime
    println("Tempo total de execução: $totalTime ms")
}
```

## Concorrência (use launch):

```kotlin
import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis

suspend fun performTask(taskId: Int) {
    delay(1000) // Simula uma tarefa assíncrona
    println("Tarefa $taskId concluída")
}

fun main() = runBlocking {
    val startTime = System.currentTimeMillis()

    val jobs = mutableListOf<Job>()
    for (i in 1..5) {
        val job = launch { performTask(i) }
        jobs.add(job)
    }

    jobs.joinAll()

    val endTime = System.currentTimeMillis()
    val totalTime = endTime - startTime
    println("Tempo total de execução: $totalTime ms")
}
```
