# Consulta-

# Documentação do Projeto — Sistema de Agendamento de Consultas SUS

🏥 Visão Geral
Este projeto simula um sistema de agendamento de consultas para hospitais públicos via SUS, permitindo que o paciente:

Faça login no sistema;

Realize o agendamento de uma consulta médica;

Visualize o local onde deverá comparecer fisicamente para a consulta.

O sistema é desenvolvido em Kotlin, voltado para aplicações Android.

🧱 Arquitetura
O projeto segue a arquitetura MVVM (Model - View - ViewModel), com integração com banco de dados simulado via Room.

Camadas:
Model: Representa as entidades como Paciente, Consulta, Hospital.

View: Interfaces gráficas (Activities e Fragments).

ViewModel: Lógica de negócios, interação com repositórios.

🗂️ Funcionalidades
Login de Paciente (com CPF e senha)

Listagem de Especialidades

Agendamento de Consulta

Visualização do Local da Consulta

🧪 Tecnologias Utilizadas
Kotlin

Android SDK

Jetpack Compose (ou XML, dependendo da UI)

Room (simulação de banco de dados local)

LiveData / ViewModel

Material Design


📁 Model: Paciente.kt
kotlin
Copiar
Editar
data class Paciente(
    val id: Int,
    val nome: String,
    val cpf: String,
    val senha: String
)
🧠 ViewModel: LoginViewModel.kt
kotlin
Copiar
Editar
class LoginViewModel : ViewModel() {

    private val pacientes = listOf(
        Paciente(1, "João Silva", "12345678900", "senha123")
    )

    fun autenticar(cpf: String, senha: String): Boolean {
        return pacientes.any { it.cpf == cpf && it.senha == senha }
    }
}
🎨 View: LoginActivity.kt
kotlin
Copiar
Editar
class LoginActivity : AppCompatActivity() {

    private val viewModel = LoginViewModel()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        val btnLogin = findViewById<Button>(R.id.btnLogin)
        btnLogin.setOnClickListener {
            val cpf = findViewById<EditText>(R.id.inputCpf).text.toString()
            val senha = findViewById<EditText>(R.id.inputSenha).text.toString()

            if (viewModel.autenticar(cpf, senha)) {
                startActivity(Intent(this, AgendamentoActivity::class.java))
            } else {
                Toast.makeText(this, "CPF ou senha inválidos", Toast.LENGTH_SHORT).show()
            }
        }
    }
}

📁 Model: Consulta.kt
kotlin
Copiar
Editar
data class Consulta(
    val id: Int,
    val pacienteId: Int,
    val especialidade: String,
    val hospital: String,
    val data: String
)
🧠 ViewModel: AgendamentoViewModel.kt
kotlin
Copiar
Editar
class AgendamentoViewModel : ViewModel() {

    private val hospitais = listOf("Hospital Central", "Posto Zona Norte", "UPA Leste")

    fun agendarConsulta(especialidade: String, pacienteId: Int): Consulta {
        val hospital = hospitais.random()
        return Consulta(
            id = (0..1000).random(),
            pacienteId = pacienteId,
            especialidade = especialidade,
            hospital = hospital,
            data = "15/06/2025 - 10h00"
        )
    }
}
🎨 View: AgendamentoActivity.kt
kotlin
Copiar
Editar
class AgendamentoActivity : AppCompatActivity() {

    private val viewModel = AgendamentoViewModel()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_agendamento)

        val btnAgendar = findViewById<Button>(R.id.btnAgendar)
        btnAgendar.setOnClickListener {
            val especialidade = "Clínico Geral" // exemplo fixo
            val consulta = viewModel.agendarConsulta(especialidade, pacienteId = 1)

            val intent = Intent(this, DetalhesConsultaActivity::class.java)
            intent.putExtra("consulta", consulta)
            startActivity(intent)
        }
    }
}

🎨 View: DetalhesConsultaActivity.kt
kotlin
Copiar
Editar
class DetalhesConsultaActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_detalhes_consulta)

        val consulta = intent.getSerializableExtra("consulta") as Consulta

        findViewById<TextView>(R.id.txtEspecialidade).text = consulta.especialidade
        findViewById<TextView>(R.id.txtHospital).text = "Local: ${consulta.hospital}"
        findViewById<TextView>(R.id.txtData).text = "Data: ${consulta.data}"
    }
}
📌 Conclusão
Este projeto simula, de maneira simples, o processo de login, agendamento e visualização do local da consulta em um sistema de saúde pública. Pode ser expandido para incluir:

Escolha de datas

Integração com APIs reais do SUS

Notificações e lembretes

Mapa de localização
