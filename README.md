# Sistema de Biblioteca
Um Sistema de Biblioteca desempenha um papel crucial no universo acadêmico, promovendo o acesso à informação e facilitando a busca por conhecimento. Essas plataformas digitais, como o sistema que desenvolvemos no projeto, representam o coração pulsante das instituições educacionais, conectando alunos, professores e entusiastas do aprendizado.

A parte central desse sistema, está o Controle de Acervo, responsável por gerenciar a vasta coleção de recursos educacionais disponíveis. Cada livro, representado por instâncias da classe Livro, guarda consigo não apenas páginas impressas, mas um mundo de ideias, histórias e conceitos. Os livros são catalogados, armazenados e disponibilizados para empréstimos, permitindo que os usuários explorem diferentes áreas do conhecimento.

A figura do Bibliotecário é essencial nesse contexto. Como guardião do conhecimento, o bibliotecário assume o papel de administrador do sistema, supervisionando empréstimos, devoluções e auxiliando os usuários na busca por informações específicas. A interação com o Controle de Empréstimo é vital para garantir que os recursos sejam distribuídos eficientemente, promovendo a circulação dinâmica dos livros.

A funcionalidade de Pesquisa é um dos pilares que torna o sistema verdadeiramente poderoso. Usuários podem explorar o vasto acervo, utilizando critérios como título, autor, ou categoria. Essa capacidade de busca facilita a localização de materiais específicos, agilizando o processo de aprendizado e pesquisa.

O Controle de Usuários, por sua vez, gerencia as informações sobre aqueles que acessam o sistema. Usuários podem se cadastrar, fornecendo informações como nome, senha e e-mail. Esse aspecto do sistema visa garantir a segurança e a personalização da experiência, permitindo que cada usuário rastreie seus empréstimos, histórico de pesquisa e atividades relacionadas à biblioteca.

O sistema não é estático; é dinâmico e adaptável. À medida que novos livros entram no acervo, o Controle de Acervo os incorpora, garantindo que a biblioteca esteja sempre atualizada. Novas funcionalidades podem ser adicionadas para aprimorar a experiência do usuário, como a implementação de uma função de recomendação com base no histórico de leitura.

A experiência do usuário é aprimorada pela interface amigável, possibilitando navegação intuitiva e rápida. O sistema se torna uma ferramenta não apenas para o aprendizado formal, mas também para a descoberta autônoma. A biblioteca digital transcende as barreiras físicas, permitindo que usuários explorem o mundo do conhecimento de qualquer lugar, a qualquer momento.


## Classes do Projeto
A incumbência da classe Bibliotecário consiste em administrar as operações inerentes a uma biblioteca, dispondo de métodos destinados ao cadastramento, atualização, empréstimo, devolução e pesquisa de livros e usuários.

### Classe Bibliotecário

No contexto da Classe Bibliotecário, as funcionalidades de cadastro de livros e de usuários proporcionam ao bibliotecário a capacidade de inserir novas informações no acervo da biblioteca. Da mesma forma, os métodos destinados à atualização de informações tanto de livros quanto de usuários conferem a flexibilidade necessária para que o bibliotecário efetue modificações quando julgar pertinente. No âmbito do gerenciamento de empréstimos, as funções de realizar empréstimo e devolução capacitam o bibliotecário a administrar eficientemente o fluxo de circulação de livros na biblioteca.

`bibliotecario.hpp`

```c++
#ifndef BIBLIOTECARIO_HPP
#define BIBLIOTECARIO_HPP

#include <iostream>
#include "ControleAcervo.hpp"
#include "ControleEmprestimo.hpp" 
#include "Pesquisa.hpp"  
#include "Usuario.hpp"

class Bibliotecario {
public:
    void cadastrarLivro(ControleAcervo& acervo);
    void atualizarInformacoesLivro(ControleAcervo& acervo, Livro& livro);
    void cadastrarUsuario(Usuario& usuario);
    void atualizarInformacoesUsuario(Usuario& usuario);
    void emitirCarteiraIdentificacao(Usuario& usuario);
    void realizarEmprestimo(ControleAcervo& acervo, ControleEmprestimo& controleEmprestimo, Usuario& usuario);
    void realizarDevolucao(ControleAcervo& acervo, ControleEmprestimo& controleEmprestimo, Usuario& usuario);
    void realizarPesquisa(ControleAcervo& acervo, Usuario& usuario, Pesquisa& pesquisa);
};

#endif // `BIBLIOTECARIO.CPP`
```

`bibliotecario.cpp`

```c++

#include "Bibliotecario.hpp"
#include "ControleEmprestimo.hpp"
#include "Pesquisa.hpp"

void Bibliotecario::cadastrarLivro(ControleAcervo &acervo) {
    std::string titulo, autor, edicao, editora, sinopse, categoria;
    int numPaginas;

    std::cout << "Digite o título do livro: ";
    std::getline(std::cin, titulo);

    std::cout << "Digite o autor do livro: ";
    std::getline(std::cin, autor);

    std::cout << "Digite a edição do livro: ";
    std::getline(std::cin, edicao);

    std::cout << "Digite a editora do livro: ";
    std::getline(std::cin, editora);

    std::cout << "Digite a sinopse do livro: ";
    std::getline(std::cin, sinopse);

    std::cout << "Digite o número de páginas do livro: ";
    std::cin >> numPaginas;

    std::cout << "Digite a categoria do livro: ";
    std::cin.ignore(); // Para limpar o buffer do teclado antes de pegar a categoria
    std::getline(std::cin, categoria);

    Livro novoLivro(titulo, autor, edicao, editora, sinopse, numPaginas, categoria);
    acervo.armazenarLivro(novoLivro);

    std::cout << "Livro cadastrado com sucesso!" << std::endl;
}

void Bibliotecario::cadastrarUsuario(Usuario &usuario) {
    // Implemente a lógica de cadastro de usuário aqui, se necessário.
}

void Bibliotecario::realizarEmprestimo(ControleAcervo &acervo, ControleEmprestimo &controleEmprestimo, Usuario &usuario) {
    // Supondo que você tenha um livro disponível no acervo
    Livro livroDisponivel = acervo.obterLivroDisponivel();

    // Chame a função registrarEmprestimo com o livro obtido
    controleEmprestimo.registrarEmprestimo(livroDisponivel, usuario, acervo);

    // Supondo que o usuário já esteja cadastrado
    std::string codigoLivro;
    std::cout << "Digite o código do livro a ser emprestado: ";
    std::cin >> codigoLivro;

    // Encontrar o livro no acervo
    Livro livroEmprestimo = acervo.buscarLivroPorCodigo(codigoLivro);

    // Realizar empréstimo
    controleEmprestimo.registrarEmprestimo(acervo, usuario, livroEmprestimo);

    std::cout << "Empréstimo realizado com sucesso!" << std::endl;
}

void Bibliotecario::realizarDevolucao(ControleAcervo &acervo, ControleEmprestimo &controleEmprestimo, Usuario &usuario) {
    // Supondo que você tenha um livro emprestado pelo usuário
    Livro livroEmprestado = controleEmprestimo.obterLivroEmprestado(usuario);

    // Chame a função registrarDevolucao com o livro obtido
    controleEmprestimo.registrarDevolucao(livroEmprestado, usuario, acervo);

    // Supondo que o usuário já tenha livros emprestados
    std::string codigoLivro;
    std::cout << "Digite o código do livro a ser devolvido: ";
    std::cin >> codigoLivro;

    // Encontrar o livro no acervo
    Livro livroDevolucao = acervo.buscarLivroPorCodigo(codigoLivro);

    // Realizar devolução
    controleEmprestimo.registrarDevolucao(acervo, usuario, livroDevolucao);

    std::cout << "Devolução realizada com sucesso!" << std::endl;
}
```
`teste_bibliotecario.cpp`

```c++

#include <gtest/gtest.h>
#include "Bibliotecario.hpp"

TEST(BibliotecarioTest, RealizarDevolucaoComProblema) {
    Bibliotecario bibliotecario;
    Usuario usuario;
    Livro livro;
    Emprestimo emprestimo;
    bibliotecario.realizarEmprestimo(emprestimo, livro, usuario);

    // Realiza a devolução com um problema
    bibliotecario.realizarDevolucao(emprestimo, "Livro danificado");

    // Verifica se o livro foi devolvido corretamente com o problema registrado
    EXPECT_TRUE(emprestimo.devolvido);
    EXPECT_EQ(emprestimo.problemaDevolucao, "Livro danificado");
}
```

### Classe Controle de Acervo

A classe ControleAcervo é responsável pelo gerenciamento do acervo de uma biblioteca. Ela fornece métodos para armazenar, buscar, emprestar, devolver e pesquisar livros.

Os métodos armazenarLivro e buscarLivroPorCodigo são responsáveis pelo gerenciamento do acervo de livros. O método armazenarLivro adiciona um livro ao acervo, enquanto o método buscarLivroPorCodigo retorna um livro do acervo pelo código.

Os métodos registrarEmprestimo e registrarDevolucao são responsáveis pelo gerenciamento de empréstimos. O método registrarEmprestimo registra um empréstimo de um livro, enquanto o método registrarDevolucao registra a devolução de um livro.

Os métodos realizarPesquisaPorTitulo e realizarPesquisaPorAutor permitem que os usuários pesquisem livros no acervo. O método realizarPesquisaPorTitulo retorna os livros do acervo que possuem o título especificado, enquanto o método realizarPesquisaPorAutor retorna os livros do acervo que possuem o autor especificado.

A classe ControleAcervo é uma classe essencial para o funcionamento de uma biblioteca. Ela permite que os bibliotecários gerenciam o acervo de forma eficiente e eficaz, atendendo às necessidades dos usuários.

`ControleAcervo.hpp`

```c++
#ifndef CONTROLEACERVO_HPP
#define CONTROLEACERVO_HPP

#include <iostream>
#include <vector>
#include <algorithm>
#include "Livro.hpp"
#include "Usuario.hpp"

class ControleAcervo {
private:
    std::vector<Livro> livros;

public:
    // Métodos relacionados ao acervo de livros
    void armazenarLivro(const Livro& livro);
    Livro buscarLivroPorCodigo(const std::string& codigo) const;
    void agruparPorGenero() const;
    void agruparPorAutor() const;

    // Métodos relacionados ao controle de empréstimo
    void registrarEmprestimo(const Livro& livro, const Usuario& usuario);
    void registrarDevolucao(const Livro& livro, const Usuario& usuario);

    // Métodos para pesquisa
    void realizarPesquisaPorTitulo(const std::string& titulo) const;
    void realizarPesquisaPorAutor(const std::string& autor) const;
};

#endif // CONTROLEACERVO_HPP
```

`ControleAcervo.cpp`

```c++
#include "ControleAcervo.hpp"

void ControleAcervo::armazenarLivro(const Livro& livro) {
    livros.push_back(livro);
}

Livro ControleAcervo::buscarLivroPorCodigo(const std::string& codigo) const {
    for (const auto& livro : livros) {
        if (livro.getCodigoCadastro() == codigo) {
            return livro;
        }
    }
    // Retornar um livro "vazio" se não encontrado (pode ser ajustado conforme necessário)
    return Livro("", "", "", "", "", "", 0, "");
}

void ControleAcervo::agruparPorGenero() const {
    // Implementar a lógica de agrupamento por gênero
    // Exemplo simples: imprimir os livros agrupados por gênero
    std::map<std::string, std::vector<Livro>> livrosPorGenero;

    for (const auto& livro : livros) {
        livrosPorGenero[livro.getCategoria()].push_back(livro);
    }

    for (const auto& [genero, livrosGenero] : livrosPorGenero) {
        std::cout << "Livros no gênero " << genero << ":\n";
        for (const auto& livro : livrosGenero) {
            livro.mostrarInformacoes();
            std::cout << "----------\n";
        }
    }
}

void ControleAcervo::agruparPorAutor() const {
    // Implementar a lógica de agrupamento por autor
    // Exemplo simples: imprimir os livros agrupados por autor
    std::map<std::string, std::vector<Livro>> livrosPorAutor;

    for (const auto& livro : livros) {
        livrosPorAutor[livro.getAutor()].push_back(livro);
    }

    for (const auto& [autor, livrosAutor] : livrosPorAutor) {
        std::cout << "Livros do autor " << autor << ":\n";
        for (const auto& livro : livrosAutor) {
            livro.mostrarInformacoes();
            std::cout << "----------\n";
        }
    }
}

void ControleAcervo::registrarEmprestimo(const Livro& livro, const Usuario& usuario) {
     // Verificar a disponibilidade do livro
    if (!livro.estaDisponivel()) {
        std::cout << "O livro não está disponível para empréstimo.\n";
        return;
    }
}

void ControleAcervo::registrarDevolucao(const Livro& livro, const Usuario& usuario) {
    // Aqui você pode verificar se o livro foi emprestado pelo usuário,
    // registrar a devolução, atualizar o estado do livro e registrar a devolução no histórico do usuário, etc.

    // Exemplo simples: marcar o livro como disponível
    livro.marcarComoDisponivel();

    // Exemplo simples: exibir mensagem de sucesso
    std::cout << "Devolução registrada com sucesso.\n";
}

void ControleAcervo::realizarPesquisaPorTitulo(const std::string& titulo) const {
    for (const auto& livro : livros) {
        if (livro.getTitulo() == titulo) {
            // Exibir informações do livro encontrado
            livro.mostrarInformacoes();
        }
    }
}

void ControleAcervo::realizarPesquisaPorAutor(const std::string& autor) const {
    for (const auto& livro : livros) {
        if (livro.getAutor() == autor) {
            // Exibir informações do livro encontrado
            livro.mostrarInformacoes();
        }
    }
}
```
`teste_acervo_pesquisa.cpp`

```c++
#include <gtest/gtest.h>
#include "Bibliotecario.hpp"

TEST(AcervoTest, AdicionarLivroAoAcervo) {
    Bibliotecario bibliotecario;
    Livro livro;

    bibliotecario.cadastrarLivro(livro);

    // Verifica se o livro foi adicionado ao acervo
    EXPECT_TRUE(/* condição de verificação */);
}

TEST(AcervoTest, PesquisarLivroPorTitulo) {
    Bibliotecario bibliotecario;
    Livro livro;
    livro.adicionarInformacoes("Senhor dos Anéis", "J.R.R. Tolkien", "1ª Edição", "Editora ABC", "Fantasia", "Uma grande jornada...", 500);

    bibliotecario.cadastrarLivro(livro);

    // Pesquisa pelo livro no acervo
    Livro livroEncontrado = bibliotecario.pesquisarLivro("Senhor dos Anéis");

    // Verifica se o livro foi encontrado corretamente
    EXPECT_EQ(livroEncontrado.getTitulo(), "Senhor dos Anéis");
    // Adicione verificações para outras informações...
}

TEST(AcervoTest, PesquisarLivroPorAutor) {
    Bibliotecario bibliotecario;
    Livro livro;
    livro.adicionarInformacoes("O Hobbit", "J.R.R. Tolkien", "1ª Edição", "Editora XYZ", "Fantasia", "Uma jornada inesperada...", 300);

    bibliotecario.cadastrarLivro(livro);

    // Pesquisa pelo livro no acervo
    Livro livroEncontrado = bibliotecario.pesquisarLivroPorAutor("J.R.R. Tolkien");

    // Verifica se o livro foi encontrado corretamente
    EXPECT_EQ(livroEncontrado.getTitulo(), "O Hobbit");
    // Adicione verificações para outras informações...
}
```

### Classe Controle de Empréstimo

A classe Controle de Empréstimo, assim, desempenha um papel crucial na garantia de que os recursos da biblioteca, representados pelos livros, sejam gerenciados de forma detalhada. Seu funcionamento eficiente contribui para a satisfação dos usuários, garantindo que os livros sejam disponibilizados quando necessário e promovendo um ciclo saudável de empréstimo e devolução.

`ControleEmprestimo.hpp`

```c++
#ifndef CONTROLEEMPRESTIMO_HPP
#define CONTROLEEMPRESTIMO_HPP

#include <iostream>
#include <vector>
#include <algorithm>
#include "Livro.hpp" 
#include "Usuario.hpp"  
#include "ControleAcervo.hpp"

class ControleEmprestimo {
private:
    std::vector<std::pair<Livro, Usuario>> emprestimos;

public:
    // Métodos relacionados ao controle de empréstimo
    void registrarEmprestimo(const Livro& livro, const Usuario& usuario, const ControleAcervo& controleAcervo);
    void registrarRenovacao(const Livro& livro, const Usuario& usuario);
    void registrarDevolucao(const Livro& livro, const Usuario& usuario, const ControleAcervo& controleAcervo);
    void gerarComprovanteDeDevolucao(const Livro& livro, const Usuario& usuario) const;
};

#endif // CONTROLEEMPRESTIMO_HPP
```

`ControleEmprestimo.cpp`

```c++
#include "ControleEmprestimo.hpp"
#include <iostream>
#include <ctime>

void ControleEmprestimo::registrarEmprestimo(const Livro& livro, const Usuario& usuario, const ControleAcervo& controleAcervo) {
    // Verificar a disponibilidade do livro no acervo
    if (!livro.estaDisponivel()) {
        std::cout << "O livro não está disponível para empréstimo.\n";
        return;
    }

    // Registrar o empréstimo no controle de acervo
    controleAcervo.registrarEmprestimo(livro, usuario);

    // Adicionar o empréstimo à lista de empréstimos
    emprestimos.push_back(std::make_pair(livro, usuario));

    // Exemplo simples: marcar o livro como indisponível
    livro.marcarComoIndisponivel();

    // Exemplo simples: exibir mensagem de sucesso
    std::cout << "Empréstimo registrado com sucesso.\n";
}

void ControleEmprestimo::registrarRenovacao(const Livro& livro, const Usuario& usuario) {
    // Verificar se o livro está emprestado para o usuário
    auto it = std::find_if(emprestimos.begin(), emprestimos.end(),
                           [&](const auto& emprestimo) {
                               return emprestimo.first.getCodigoCadastro() == livro.getCodigoCadastro() &&
                                      emprestimo.second.getNomeUsuario() == usuario.getNomeUsuario();
                           });

    if (it != emprestimos.end()) {
        // Aqui você pode adicionar a lógica para verificar se é possível renovar
        // e atualizar a data de devolução, se necessário.

        // Exemplo simples: exibir mensagem de sucesso
        std::cout << "Renovação registrada com sucesso.\n";
    } else {
        std::cout << "Livro não emprestado para o usuário. Não é possível renovar.\n";
    }
}

void ControleEmprestimo::registrarDevolucao(const Livro& livro, const Usuario& usuario, const ControleAcervo& controleAcervo) {
    // Verificar se o livro foi emprestado pelo usuário
    auto it = std::find_if(emprestimos.begin(), emprestimos.end(),
                           [&](const auto& emprestimo) {
                               return emprestimo.first.getCodigoCadastro() == livro.getCodigoCadastro() &&
                                      emprestimo.second.getNomeUsuario() == usuario.getNomeUsuario();
                           });

    if (it != emprestimos.end()) {
        // Registrar a devolução no controle de acervo
        controleAcervo.registrarDevolucao(livro, usuario);

        // Remover o empréstimo da lista de empréstimos
        emprestimos.erase(it);

        // Exemplo simples: marcar o livro como disponível
        livro.marcarComoDisponivel();

        // Exemplo simples: exibir mensagem de sucesso
        std::cout << "Devolução registrada com sucesso.\n";
    } else {
        std::cout << "Livro não emprestado para o usuário. Não é possível devolver.\n";
    }
}
	// Implementar a lógica para gerar um comprovante de devolução
	// Função auxiliar para obter a data atual
	std::string getCurrentDate() {
    	std::time_t now = std::time(0);
    	std::tm* localTime = std::
    	st
	localtime(&now);
    	char buffer[80];
   	 std::strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", localTime);
    	return buffer;
	}

void ControleEmprestimo::gerarComprovanteDeDevolucao(const Livro& livro, const Usuario& usuario) const {
    
    // Pode incluir informações como livro, usuário, data de devolução, etc.
    std::cout << "Comprovante de Devolução:\n";
    std::cout << "Livro: " << livro.getTitulo() << "\n";
    std::cout << "Usuário: " << usuario.getNomeUsuario() << "\n";
    std::cout << "Data de Devolução: " << getCurrentDate() << "\n"; // Implemente uma função para obter a data atual
}
```
`teste_emprestimo.cpp`

```c++
#include <gtest/gtest.h>
#include "Emprestimo.hpp"

TEST(EmprestimoTest, CalcularDataDevolucao) {
    Emprestimo emprestimo;
    emprestimo.registrarEmprestimo("L123", "U456");

    // Verifica se a data de devolução foi calculada corretamente
    // Implemente as verificações conforme necessário
    EXPECT_TRUE(/* condição de verificação */);
}

TEST(EmprestimoTest, RenovarEmprestimoAposDataLimite) {
    Emprestimo emprestimo;
    emprestimo.registrarEmprestimo("L123", "U456");

    // Simula a passagem do tempo para além da data de devolução
    // Implemente as verificações conforme necessário
    EXPECT_FALSE(emprestimo.podeRenovar());
}
```

### Classe Livro

A Classe Livro é uma parte essencial do nosso projeto de Sistema de Biblioteca. Ela representa as informações fundamentais de um livro em nosso acervo. Cada instância da Classe Livro contém dados como código, título, autor, edição, editora, sinopse, número de páginas e gênero.

`Livro.hpp`

```c++
#ifndef LIVRO_HPP
#define LIVRO_HPP

#include <iostream>
#include <string>

class Livro {
private:
    std::string codigoCadastro;
    std::string titulo;
    std::string autor;
    std::string edicao;
    std::string editora;
    std::string sinopse;
    int numPaginas;
    std::string categoria;
    bool disponivel;

public:
    Livro(std::string codigo, std::string t, std::string a, std::string e, std::string ed, std::string s, int np, std::string cat);
    
    // Métodos de acesso às informações do livro
    std::string getCodigoCadastro() const;
    std::string getTitulo() const;
    std::string getAutor() const;
    std::string getEdicao() const;
    std::string getEditora() const;
    std::string getSinopse() const;
    int getNumPaginas() const;
    std::string getCategoria() const;
    bool estaDisponivel() const;

    // Métodos para atualizar informações do livro
    void mostrarInformacoes() const;
    void atualizarInformacoes();
    void marcarComoDisponivel();
    void marcarComoIndisponivel();
    void atualizarTitulo(const std::string& novoTitulo);
    void atualizarSinopse(const std::string& novaSinopse);
};

#endif
```

`Livro.cpp`

```c++
#include "Livro.hpp"

Livro::Livro(std::string codigo, std::string t, std::string a, std::string e, std::string ed, std::string s, int np, std::string cat)
    : codigoCadastro(codigo), titulo(t), autor(a), edicao(e), editora(ed), sinopse(s), numPaginas(np), categoria(cat), disponivel(true) {}

std::string Livro::getCodigoCadastro() const {
    return codigoCadastro;
}

std::string Livro::getTitulo() const {
    return titulo;
}

std::string Livro::getAutor() const {
    return autor;
}

std::string Livro::getGenero() const {
    return this->genero;
}


std::string Livro::getEdicao() const {
    return edicao;
}

std::string Livro::getEditora() const {
    return editora;
}

std::string Livro::getSinopse() const {
    return sinopse;
}

int Livro::getNumPaginas() const {
    return numPaginas;
}

std::string Livro::getCategoria() const {
    return categoria;
}

bool Livro::estaDisponivel() const {
    return disponivel;
}

void Livro::mostrarInformacoes() const {
    std::cout << "Código de Cadastro: " << codigoCadastro << std::endl;
    std::cout << "Título: " << titulo << std::endl;
    std::cout << "Autor: " << autor << std::endl;
    std::cout << "Edição: " << edicao << std::endl;
    std::cout << "Editora: " << editora << std::endl;
    std::cout << "Sinopse: " << sinopse << std::endl;
    std::cout << "Número de Páginas: " << numPaginas << std::endl;
    std::cout << "Categoria: " << categoria << std::endl;
    std::cout << "Disponível: " << (disponivel ? "Sim" : "Não") << std::endl;
}

void Livro::atualizarTitulo(const std::string& novoTitulo) {
    this->titulo = novoTitulo;
}

void Livro::atualizarSinopse(const std::string& novaSinopse) {
    this->sinopse = novaSinopse;
}

void Livro::marcarComoDisponivel() {
    disponivel = true;
}

void Livro::marcarComoIndisponivel() {
    disponivel = false;
}

void exibirResultados(const std::vector<Livro>& resultados) {
    // Implementação da função para exibir os resultados
    for (const Livro& livro : resultados) {
        std::cout << "Título: " << livro.getTitulo() << std::endl;
        // Adicione mais informações do livro conforme necessário
    }
}
```
`teste_livro.cpp`

```c++
#include <gtest/gtest.h>
#include "Livro.hpp"

TEST(LivroTest, AdicionarLivro) {
    Livro livro;
    livro.adicionarInformacoes("Senhor dos Anéis", "J.R.R. Tolkien", "1ª Edição", "Editora ABC", "Fantasia", "Uma grande jornada...", 500);

    // Verifica se as informações foram adicionadas corretamente
    EXPECT_EQ(livro.getTitulo(), "Senhor dos Anéis");
    // Adicione verificações para outras informações...
}

TEST(LivroTest, AtualizarInformacoes) {
    Livro livro;
    livro.adicionarInformacoes("Senhor dos Anéis", "J.R.R. Tolkien", "1ª Edição", "Editora ABC", "Fantasia", "Uma grande jornada...", 500);

    // Atualiza as informações do livro
    livro.atualizarInformacoes("Senhor dos Anéis", "J.R.R. Tolkien", "2ª Edição", "Nova Editora", "Fantasia Épica", "Uma jornada épica...", 600);

    // Verifica se as informações foram atualizadas corretamente
    EXPECT_EQ(livro.getEdicao(), "2ª Edição");
    // Adicione verificações para outras informações...
}
```

### Classe Pesquisa

Uma Classe Pesquisa é uma parte essencial do nosso sistema de biblioteca, oferecendo uma funcionalidade robusta para encontrar livros dentro do vasto acervo da biblioteca. Seu principal objetivo é simplificar o processo de busca, permitindo que usuários, bibliotecários e outros específicos encontrem rapidamente obras específicas com base em diferentes critérios.

`Pesquisa.hpp`

```c++
#ifndef PESQUISA_HPP
#define PESQUISA_HPP

#include <string>
#include <iostream>
#include <vector>
#include "Livro.hpp"
#include "ControleAcervo.hpp"

class Pesquisa {
public:
    // Métodos
    std::vector<Livro> pesquisarPorTitulo(const std::string& titulo, const ControleAcervo& controleAcervo) const;
    std::vector<Livro> pesquisarPorAutor(const std::string& autor, const ControleAcervo& controleAcervo) const;
    std::vector<Livro> pesquisarPorGenero(const std::string& genero, const ControleAcervo& controleAcervo) const;
};

#endif // PESQUISA_HPP
```

`Pesquisa.cpp`

```c++
#include "Pesquisa.hpp"

std::vector<Livro> Pesquisa::pesquisarPorTitulo(const std::string& titulo, const ControleAcervo& controleAcervo) const {
    // Lógica para pesquisar por título
    return controleAcervo.pesquisarPorTitulo(titulo);
}

std::vector<Livro> Pesquisa::pesquisarPorAutor(const std::string& autor, const ControleAcervo& controleAcervo) const {
    // Lógica para pesquisar por autor
    return controleAcervo.pesquisarPorAutor(autor);
}

std::vector<Livro> Pesquisa::pesquisarPorGenero(const std::string& genero, const ControleAcervo& controleAcervo) const {
    // Lógica para pesquisar por gênero
    return controleAcervo.pesquisarPorGenero(genero);
}
```

### Classe Usuário

A Classe Usuário desempenha um papel essencial no contexto do nosso sistema de biblioteca. Esta classe é responsável por representar os usuários que interagem com a biblioteca, como alunos, professores ou qualquer pessoa que deseje utilizar os serviços oferecidos.

```c++
#ifndef USUARIO_HPP
#define USUARIO_HPP

#include <string>
#include <vector>

class Usuario {
private:
    std::string nomeUsuario;
    std::string senha;
    std::string email;
    std::vector<std::pair<std::string, std::string>> historicoEmprestimos;  // Par (Código do Livro, Data de Empréstimo)

public:
    // Construtor
    Usuario(const std::string& nome, const std::string& senha, const std::string& email);

    // Métodos
    const std::string& getNomeUsuario() const;
    const std::string& getSenha() const;
    const std::string& getEmail() const;
    void mostrarHistorico() const;
    void adicionarHistorico(const std::string& codigoLivro, const std::string& dataEmprestimo);
};

#endif
```

`Usuario.cpp`

```c++
#include "Usuario.hpp"
#include <iostream>

// Construtor
Usuario::Usuario(const std::string& nome, const std::string& senha, const std::string& email)
    : nomeUsuario(nome), senha(senha), email(email) {}

// Métodos
const std::string& Usuario::getNomeUsuario() const {
    return nomeUsuario;
}

const std::string& Usuario::getSenha() const {
    return senha;
}

const std::string& Usuario::getEmail() const {
    return email;
}

void Usuario::mostrarHistorico() const {
    std::cout << "Histórico de Empréstimos do Usuário " << nomeUsuario << ":\n";
    for (const auto& emprestimo : historicoEmprestimos) {
        std::cout << "Livro: " << emprestimo.first << ", Data de Empréstimo: " << emprestimo.second << "\n";
    }
    std::cout << "-------------------------------------\n";
}

void Usuario::adicionarHistorico(const std::string& codigoLivro, const std::string& dataEmprestimo) {
    historicoEmprestimos.push_back(std::make_pair(codigoLivro, dataEmprestimo));
}
```
`teste_usuario.cpp`

```c++
#include <gtest/gtest.h>
#include "Usuario.hpp"

TEST(UsuarioTest, RenovarEmprestimoInexistente) {
    Usuario usuario;
    Emprestimo emprestimo;
    usuario.historicoEmprestimos.push_back(emprestimo);

    // Tenta renovar um empréstimo que não existe
    usuario.renovarEmprestimo("CodigoInvalido");

    // Verifica se o empréstimo não foi renovado
    EXPECT_FALSE(emprestimo.podeRenovar());
}

TEST(UsuarioTest, ExibirHistoricoVazio) {
    Usuario usuario;

    // Verifica se o histórico é exibido corretamente quando vazio
    testing::internal::CaptureStdout();
    usuario.exibirHistorico();
    std::string output = testing::internal::GetCapturedStdout();
    EXPECT_TRUE(output.empty());
}
```

## MAIN

```c++
#include "Bibliotecario.hpp"
#include "Livro.hpp"
#include "ControleAcervo.hpp"
#include "ControleEmprestimo.hpp"
#include "Usuario.hpp"
#include "Pesquisa.hpp"
#include <iostream>
#include <vector>

int main() {
    // Declarar e inicializar objetos
    Bibliotecario bibliotecario;
    Usuario usuario("NomeUsuario", "Senha123", "usuario@email.com");
    ControleAcervo acervo;
    ControleEmprestimo controleEmprestimo;
    Pesquisa pesquisa;
    ControleAcervo controleAcervo;

    // Adicionar um exemplo de cadastro de usuário
    bibliotecario.cadastrarUsuario(usuario);

    // Exemplo de realização de empréstimo
    bibliotecario.realizarEmprestimo(acervo, controleEmprestimo, usuario);

    // Exemplo de realização de devolução
    bibliotecario.realizarDevolucao(acervo, controleEmprestimo, usuario);

    // Exemplo de realização de pesquisa
    bibliotecario.realizarPesquisa(acervo, usuario, pesquisa);

    // Adicionando um exemplo de armazenamento de livro
    controleAcervo.armazenarLivro(Livro("Codigo123", "TituloLivro", "AutorLivro", "Edicao1", "EditoraXYZ", "Sinopse...", 200, "Ficcao"));

    return 0;
}

//Livro//

    // Exemplo de criação de livros
	Livro livro1("123ABC", "Aventuras no Mundo da Programação", "Autor Exemplo", "1ª Edição", "Editora Livro Bom", "Sinopse do Livro", 200, "Programação");
	Livro livro2("456DEF", "Introdução à Inteligência Artificial", "Autor AI", "3ª Edição", "Editora Tech AI", "Sinopse de AI", 300, "Inteligência Artificial");
	Livro livro3("456DEF", "História Fascinante", "Escritor Imaginário", "2ª Edição", "Editora História Viva", "Sinopse do Livro", 250, "História");
	Livro livro4("789GHI", "O Mistério do Universo", "Autor Desconhecido", "3ª Edição", "Editora Astral", "Sinopse do Livro", 180, "Ciências");
	Livro livro5("101JKL", "Amor nas Estrelas", "Romancista Sonhador", "1ª Edição", "Editora Romances Eternos", "Sinopse do Livro", 300, "Romance");
	Livro livro6("112MNO", "Aventuras na Selva", "Aventureiro Intrépido", "2ª Edição", "Editora Explorando", "Sinopse do Livro", 220, "Aventura");
	Livro livro7("131PQR", "Receitas do Mundo", "Chef Renomado", "3ª Edição", "Editora Gastronomia Global", "Sinopse do Livro", 150, "Culinária");
	Livro livro8("415STU", "Poesias do Coração", "Poeta Sensível", "1ª Edição", "Editora Versos Livres", "Sinopse do Livro", 180, "Poesia");
	Livro livro9("161VWX", "Segredos do Passado", "Historiador Misterioso", "2ª Edição", "Editora Enigmas", "Sinopse do Livro", 250, "História");
	Livro livro10("718YZA", "A Arte da Fotografia", "Fotógrafo Talentoso", "1ª Edição", "Editora Imagens Perfeitas", "Sinopse do Livro", 220, "Arte");
	Livro livro11("192BCD", "Viagem ao Centro da Terra", "Aventureiro Intrépido", "1ª Edição", "Editora Explorando", "Sinopse do Livro", 280, "Aventura");




    // Exemplo de exibição de informações dos livros
    std::cout << "Informações do Livro 1:\n";
    livro1.mostrarInformacoes();
    std::cout << "\nInformações do Livro 2:\n";
    livro2.mostrarInformacoes();
    std::cout << "\nInformações do Livro 3:\n";
    livro3.mostrarInformacoes();
    std::cout << "\nInformações do Livro 4:\n";
    livro4.mostrarInformacoes();
    std::cout << "\nInformações do Livro 5:\n";
    livro5.mostrarInformacoes();
    std::cout << "\nInformações do Livro 6:\n";
    livro6.mostrarInformacoes();
    std::cout << "\nInformações do Livro 7:\n";
    livro7.mostrarInformacoes();
    std::cout << "\nInformações do Livro 8:\n";
    livro8.mostrarInformacoes();
    std::cout << "\nInformações do Livro 9:\n";
    livro9.mostrarInformacoes();
    std::cout << "\nInformações do Livro 10:\n";
    livro10.mostrarInformacoes();
    std::cout << "\nInformações do Livro 11:\n";
    livro11.mostrarInformacoes();

    // Exemplo de atualização de informações do livro
    livro1.atualizarTitulo("Aventuras no Mundo da Programação - Edição Atualizada");
    livro1.atualizarSinopse("Nova Sinopse Atualizada");

    // Exibir informações atualizadas
    std::cout << "\nInformações atualizadas do Livro 1:\n";
    livro1.mostrarInformacoes();

    // Exemplo de marcação de disponibilidade
    livro2.marcarComoIndisponivel();

    // Exibir informações atualizadas
    std::cout << "\nInformações atualizadas do Livro 2:\n";
    livro2.mostrarInformacoes();

    return 0;

//ControleAcervo//

    // Criando um objeto do ControleAcervo
	ControleAcervo controleAcervo;
	controleAcervo.armazenarLivro(livro1);
	controleAcervo.armazenarLivro(livro2);
	controleAcervo.armazenarLivro(livro3);
	controleAcervo.armazenarLivro(livro4);
	controleAcervo.armazenarLivro(livro5);
	controleAcervo.armazenarLivro(livro6);
	controleAcervo.armazenarLivro(livro7);
	controleAcervo.armazenarLivro(livro8);
	controleAcervo.armazenarLivro(livro9);
	controleAcervo.armazenarLivro(livro10);
	controleAcervo.armazenarLivro(livro11);

    // Exemplo de busca de livro por código
    Livro livroEncontrado = acervo.buscarLivroPorCodigo("123ABC");

    // Exemplo de pesquisa por título
    acervo.realizarPesquisaPorTitulo("Aventuras no Mundo da Programação");

    // Exemplo de agrupamento por autor
    acervo.agruparPorAutor();

    return 0;

//ControleEmprestimo//


    // Criar objeto ControleAcervo e adicionar o livro ao acervo
    ControleAcervo controleAcervo;
    controleAcervo.armazenarLivro(livro);

    // Criar objeto ControleEmprestimo
    ControleEmprestimo controleEmprestimo;

    // Registrar um empréstimo
    controleEmprestimo.registrarEmprestimo(livro, usuario, controleAcervo);

    // Registrar uma renovação
    controleEmprestimo.registrarRenovacao(livro, usuario);

    // Registrar uma devolução
    controleEmprestimo.registrarDevolucao(livro, usuario, controleAcervo);

    // Gerar comprovante de devolução
    controleEmprestimo.gerarComprovanteDeDevolucao(livro, usuario);

    return 0;

//Usuario//

    // Criar objeto de exemplo
    Usuario usuario("usuario123", "senha123", "email@exemplo.com");

    // Exibir informações do usuário
    std::cout << "Nome do Usuário: " << usuario.getNomeUsuario() << "\n";
    std::cout << "Senha: " << usuario.getSenha() << "\n";
    std::cout << "Email: " << usuario.getEmail() << "\n";

    // Adicionar alguns empréstimos ao histórico
    usuario.adicionarHistorico("123ABC", "2023-01-01");
    usuario.adicionarHistorico("456DEF", "2023-02-01");

    // Mostrar o histórico de empréstimos
    usuario.mostrarHistorico();

    return 0;
#include "Pesquisa.hpp"
#include <iostream>
#include <vector>

// Função auxiliar para exibir resultados da pesquisa
void exibirResultados(const std::vector<Livro>& resultados);

int main() {
    // Criar objeto ControleAcervo e adicionar os livros ao acervo
    ControleAcervo controleAcervo;
    controleAcervo.armazenarLivro(livro1);
    controleAcervo.armazenarLivro(livro2);
    controleAcervo.armazenarLivro(livro3);
    controleAcervo.armazenarLivro(livro4);
    controleAcervo.armazenarLivro(livro5);
    controleAcervo.armazenarLivro(livro6);
    controleAcervo.armazenarLivro(livro7);
    controleAcervo.armazenarLivro(livro8);
    controleAcervo.armazenarLivro(livro9);
    controleAcervo.armazenarLivro(livro10);
    controleAcervo.armazenarLivro(livro11);

    // Criar objeto Pesquisa
    Pesquisa pesquisa;

    // Realizar pesquisas
    std::cout << "Pesquisa por Título:\n";
    std::vector<Livro> resultadoTitulo = pesquisa.pesquisarPorTitulo("Aventuras", controleAcervo);
    exibirResultados(resultadoTitulo);

    std::cout << "\nPesquisa por Autor:\n";
    std::vector<Livro> resultadoAutor = pesquisa.pesquisarPorAutor("Autor Exemplo", controleAcervo);
    exibirResultados(resultadoAutor);

    std::cout << "\nPesquisa por Gênero:\n";
    std::vector<Livro> resultadoGenero = pesquisa.pesquisarPorGenero("Ficção Científica", controleAcervo);
    exibirResultados(resultadoGenero);

    return 0;
}

// Função auxiliar para exibir resultados da pesquisa
void exibirResultados(const std::vector<Livro>& resultados) {
    if (resultados.empty()) {
        std::cout << "Nenhum resultado encontrado.\n";
    } else {
        for (const auto& livro : resultados) {
            std::cout << "Livro: " << livro.getTitulo() << " | Autor: " << livro.getAutor() << " | Gênero: " << livro.getGenero() << "\n";
        }
    }
}

