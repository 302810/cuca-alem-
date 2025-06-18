fun main() {
    val usuarios = mutableMapOf<String, String>()
    val carrinho = mutableListOf<Cuca>()

    val sabores = listOf(
        Cuca("Abacaxi", 35.0),
        Cuca("Farofa", 30.0),
        Cuca("Uva", 30.0),
        Cuca("Requeijão", 35.0),
        Cuca("Chocolate", 35.0)
    )

    println("🧁 Bem-vindo ao sistema da Cuca Alemã!")
    while (true) {
        println("\n1 - Login")
        println("2 - Cadastrar")
        println("3 - Sair")
        print("Escolha uma opção: ")
        when (readLine()?.trim()) {
            "1" -> {
                print("Usuário: ")
                val user = readLine()!!
                print("Senha: ")
                val pass = readLine()!!
                if (usuarios[user] == pass) {
                    println("Login bem-sucedido. Olá, $user!")
                    menuPrincipal(sabores, carrinho)
                } else {
                    println("Usuário ou senha incorretos.")
                }
            }
            "2" -> {
                print("Novo usuário: ")
                val newUser = readLine()!!
                print("Nova senha: ")
                val newPass = readLine()!!
                usuarios[newUser] = newPass
                println("Usuário cadastrado com sucesso!")
            }
            "3" -> {
                println("Até logo!")
                return
            }
            else -> println("Opção inválida.")
        }
    }
}

data class Cuca(val sabor: String, val preco: Double)

fun menuPrincipal(sabores: List<Cuca>, carrinho: MutableList<Cuca>) {
    while (true) {
        println("\n🍰 Sabores disponíveis:")
        sabores.forEachIndexed { index, cuca ->
            println("${index + 1} - ${cuca.sabor} (R$ ${"%.2f".format(cuca.preco)})")
        }
        println("${sabores.size + 1} - Ver Carrinho")
        println("${sabores.size + 2} - Finalizar Pedido")
        println("${sabores.size + 3} - Logout")

        print("Escolha uma opção: ")
        val escolha = readLine()?.toIntOrNull()

        when {
            escolha in 1..sabores.size -> {
                val selecionada = sabores[escolha!! - 1]
                carrinho.add(selecionada)
                println("${selecionada.sabor} adicionada ao carrinho.")
            }
            escolha == sabores.size + 1 -> {
                mostrarCarrinho(carrinho)
            }
            escolha == sabores.size + 2 -> {
                finalizarPedido(carrinho)
                break
            }
            escolha == sabores.size + 3 -> {
                println("Saindo para login.")
                break
            }
            else -> println("Opção inválida.")
        }
    }
}

fun mostrarCarrinho(carrinho: List<Cuca>) {
    println("\n🛒 Carrinho:")
    if (carrinho.isEmpty()) {
        println("Carrinho vazio.")
        return
    }

    carrinho.groupingBy { it.sabor }
        .eachCount()
        .forEach { (sabor, qtd) ->
            println("$sabor x$qtd")
        }

    val total = carrinho.sumOf { it.preco }
    println("Total: R$ ${"%.2f".format(total)}")
}

fun finalizarPedido(carrinho: MutableList<Cuca>) {
    if (carrinho.isEmpty()) {
        println("Carrinho está vazio.")
        return
    }
    println("\n✅ Pedido Finalizado!")
    mostrarCarrinho(carrinho)
    carrinho.clear()
    println("Obrigado por comprar na Cuca Alemã!")
}
