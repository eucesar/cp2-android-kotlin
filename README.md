# 2° CP da materia Android Kotlin Developer do professor Ewerton Luiz de Lima Carreira

```
Informações do Aluno :
Cesar Iglesias Balseiro Neto - RM : 98007
```

# Projeto CryptoMonitor

Este projeto foi obtido diretamente do repositório GitHub do professor da disciplina de Desenvolvimento Android. O objetivo deste trabalho é descrever detalhadamente o funcionamento do projeto com o intuito de garantir nota, mostrando o entendimento de cada parte do código Kotlin envolvido.

## 📱 Objetivo do Projeto

O aplicativo tem como principal funcionalidade exibir ao usuário o **último valor de cotação do Bitcoin (BTC)** utilizando dados fornecidos pela **API do Mercado Bitcoin**. Ao clicar no botão "Refresh", o app faz uma chamada à API e atualiza as informações na tela, mostrando:

- O **último valor do Bitcoin**
- A **data e hora da cotação**

Esse projeto serve como exemplo prático de uso de **Retrofit**, **Kotlin**, **Coroutines** e manipulação de dados em um app Android.



## 📂 Estrutura do Projeto e Explicação dos Arquivos

### `model/TickerResponse.kt`

```kotlin
package carreiras.com.github.cryptomonitor.model
```

## 📱 Resultado final :

<p align="center"> 
  <img src="https://github.com/user-attachments/assets/9d6ca7ac-d5cf-4d54-83e6-0c25212794d7" alt="Resultado final do app" />
</p>

## ⌨️ Evidencias Codigos :

<p align="center"> TickerResponse.kt <p/>
<p align="center">
  <img src="https://github.com/user-attachments/assets/93b68af2-28e3-45f2-91e2-c2705e9cecc9" alt="Tela 1 do app" />
</p>

<p align="center"> MercadoBitcoinService.kt <p/>
<p align="center">
  <img src="https://github.com/user-attachments/assets/d8593a58-399c-4fb0-b9ad-dbda0844ede6" alt="Tela 2 do app" />
</p>

<p align="center"> MercadoBitcoinServiceFactory.kt <p/>
<p align="center">
  <img src="https://github.com/user-attachments/assets/f1ee4031-72da-42e1-bcdd-9dccac5b785d" alt="Tela 3 do app" />
</p>

<p align="center"> MainActivity.kt <p/>
<p align="center">
  <img src="https://github.com/user-attachments/assets/c5d0d90a-5444-4698-a54c-946b8c5210fb" alt="Tela 4 do app - Parte 1" />
  <br />
  <img src="https://github.com/user-attachments/assets/e4f91d74-3b51-4c97-89af-92cb68e2604a" alt="Tela 4 do app - Parte 2" />
</p>

## Arquivo: TickerResponse.kt

```kotlin
package carreiras.com.github.cryptomonitor.model
```
- Define o pacote onde esse arquivo está localizado, utilizado para organização do código.

```kotlin
class TickerResponse(
    val ticker: Ticker
)
```
- Declara a classe TickerResponse, que representa a resposta principal da API do Mercado Bitcoin.
- O construtor da classe possui uma única propriedade chamada ticker, do tipo Ticker, que representa os dados da cotação do Bitcoin.

```kotlin
class Ticker(
    val high: String,
    val low: String,
    val vol: String,
    val last: String,
    val buy: String,
    val sell: String,
    val date: Long
)
```
- Declara a classe Ticker, que representa os dados internos da cotação.
- high: maior valor do Bitcoin no período.
- low: menor valor do período.
- vol: volume negociado.
- last: último valor negociado (esse é o que será exibido no app).
- buy: preço de compra.
- sell: preço de venda.
- date: timestamp da cotação (em segundos desde 1970).

## Arquivo: MercadoBitcoinService.kt
```kotlin
package carreiras.com.github.cryptomonitor.service
```
- Define o pacote onde a interface de serviço da API está localizada.

```kotlin
import carreiras.com.github.cryptomonitor.model.TickerResponse
import retrofit2.Response
import retrofit2.http.GET
```
- Importa a classe TickerResponse (modelo de resposta).
- Importa a classe Response do Retrofit, que representa a resposta HTTP.
- Importa a anotação @GET usada para fazer requisições HTTP do tipo GET.

```kotlin
interface MercadoBitcoinService {
```
- Define uma interface que representa o contrato da comunicação com a API.

```kotlin
@GET("api/BTC/ticker/")
```
- Anotação que indica que esse método faz uma requisição GET para o endpoint api/BTC/ticker/.

```kotlin
    suspend fun getTicker(): Response<TickerResponse>
}
```
- Declara a função getTicker, marcada como suspend para ser chamada dentro de uma coroutine.
- Retorna um Response<TickerResponse>, ou seja, uma resposta da API contendo os dados da cotação.

## Arquivo: MercadoBitcoinServiceFactory.kt
```kotlin
package carreiras.com.github.cryptomonitor.service
```
- Define o pacote para essa classe, responsável por criar o Retrofit.

```kotlin
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
```
- Importa o Retrofit, que é usado para fazer chamadas HTTP.
- Importa o GsonConverterFactory, que converte JSON em objetos Kotlin automaticamente.

```kotlin
class MercadoBitcoinServiceFactory {
```
- Declara a classe responsável por criar instâncias do MercadoBitcoinService.

```kotlin
fun create(): MercadoBitcoinService {
```
- Função pública que retorna uma instância da interface MercadoBitcoinService.

```kotlin
val retrofit = Retrofit.Builder()
   .baseUrl("https://www.mercadobitcoin.net/")
```
- Cria uma instância do Retrofit com o baseUrl apontando para a URL base da API.

```kotlin
.addConverterFactory(GsonConverterFactory.create())
```
- Adiciona um conversor de JSON (usando Gson) para que a resposta seja transformada em objetos Kotlin.

```kotlin
.build()
```
- Finaliza a construção do objeto Retrofit.

```kotlin
        return retrofit.create(MercadoBitcoinService::class.java)
    }
}
```
- Retorna uma instância concreta da interface MercadoBitcoinService para ser usada nas chamadas de rede.

## Arquivo: MainActivity.kt
```kotlin
package carreiras.com.github.cryptomonitor
```
- Define o pacote principal da aplicação.

```kotlin
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.appcompat.widget.Toolbar
```
- Importa as bibliotecas Android necessárias para criação da interface, botões, mensagens, toolbar e controle da Activity.

```kotlin
import carreiras.com.github.cryptomonitor.service.MercadoBitcoinServiceFactory
```
- Importa a classe MercadoBitcoinServiceFactory, responsável por criar o serviço de acesso à API.

```kotlin
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
```
- Importa as ferramentas de coroutines, usadas para executar tarefas assíncronas (como chamadas de rede).

```kotlin
import java.text.NumberFormat
import java.text.SimpleDateFormat
import java.util.Date
import java.util.Locale
```
- Importa utilitários para formatar números e datas conforme a localidade.

```kotlin
class MainActivity : AppCompatActivity() {
```
- Declara a MainActivity, que é a tela principal do app e herda de AppCompatActivity.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
```
- Sobrescreve o método onCreate, chamado quando a Activity é criada.

```kotlin
super.onCreate(savedInstanceState)
setContentView(R.layout.activity_main)
```
- Chama o super para executar o comportamento padrão da Activity.
- Define o layout da tela principal (activity_main.xml).


```kotlin
        val toolbarMain: Toolbar = findViewById(R.id.toolbar_main)
        configureToolbar(toolbarMain)
```
- Busca a Toolbar no layout e passa para o método de configuração.

```kotlin
        val btnRefresh: Button = findViewById(R.id.btn_refresh)
        btnRefresh.setOnClickListener {
            makeRestCall()
        }
```
- Busca o botão de "Refresh" no layout.
- Adiciona um ouvinte de clique para executar a função makeRestCall() quando o botão for clicado.

```kotlin
    private fun configureToolbar(toolbar: Toolbar) {
```
- Função que configura o visual e título da toolbar.

```kotlin
           setSupportActionBar(toolbar)
        toolbar.setTitleTextColor(getColor(R.color.white))
        supportActionBar?.setTitle(getText(R.string.app_title))
        supportActionBar?.setBackgroundDrawable(getDrawable(R.color.primary))
    }
```
- Define a toolbar como a barra de ações da Activity.
- Define a cor do texto como branca.
- Define o título usando o texto do arquivo de strings.
- Define o fundo da toolbar com a cor primária.

```kotlin
    private fun makeRestCall() {
        CoroutineScope(Dispatchers.Main).launch {
```
- Cria uma coroutine no escopo principal (UI thread).
- Dentro dela será feita a chamada de rede.

```kotlin
            try {
                val service = MercadoBitcoinServiceFactory().create()
                val response = service.getTicker()
```
- Cria o serviço de rede e faz a chamada para pegar os dados do ticker.

```kotlin
                if (response.isSuccessful) {
                    val tickerResponse = response.body()
```
- Verifica se a resposta foi bem-sucedida.
- Extrai o corpo da resposta em um objeto tickerResponse.

```kotlin
                    val lblValue: TextView = findViewById(R.id.lbl_value)
                    val lblDate: TextView = findViewById(R.id.lbl_date)
```
- Encontra os campos de texto na tela para exibir os valores.

```kotlin
                    val lastValue = tickerResponse?.ticker?.last?.toDoubleOrNull()
```
- Pega o valor mais recente da cotação e converte para Double.

```kotlin
                    if (lastValue != null) {
                        val numberFormat = NumberFormat.getCurrencyInstance(Locale("pt", "BR"))
                        lblValue.text = numberFormat.format(lastValue)
                    }
```
- Se o valor for válido, formata como moeda brasileira e exibe no campo de texto.

```kotlin
                    val date = tickerResponse?.ticker?.date?.let { Date(it * 1000L) }
                    val sdf = SimpleDateFormat("dd/MM/yyyy HH:mm:ss", Locale.getDefault())
                    lblDate.text = sdf.format(date)
```
- Converte o timestamp da API em um objeto Date.
- Formata a data para o formato "dd/MM/yyyy HH:mm:ss".
- Exibe a data formatada no campo de texto.

```kotlin
                } else {
                    val errorMessage = when (response.code()) {
                        400 -> "Bad Request"
                        401 -> "Unauthorized"
                        403 -> "Forbidden"
                        404 -> "Not Found"
                        else -> "Unknown error"
                    }
                    Toast.makeText(this@MainActivity, errorMessage, Toast.LENGTH_LONG).show()
                }
```
- Caso a resposta não seja bem-sucedida, exibe uma mensagem de erro conforme o código de status.

```kotlin
            } catch (e: Exception) {
                Toast.makeText(this@MainActivity, "Falha na chamada: ${e.message}", Toast.LENGTH_LONG).show()
            }
        }
    }
}

```
- Captura erros que podem acontecer durante a chamada e exibe uma Toast com a mensagem de falha.
