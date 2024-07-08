const prompt = require('prompt-sync')();

console.log("Seja bem-vindo(a) ao seu diário pessoal");
console.log(" ");

let atividades = [];
let categorias = ["Trabalho", "Estudo", "Lazer"];

function adicionarAtividade() {
    console.log("Digite o nome da atividade:");
    let nomeAtividade = prompt(" ");

    let dataAtividade;
    let dataValida = false;
    do {
        console.log("Digite a data da atividade (no formato 'yyyy-mm-dd'):");
        dataAtividade = prompt(" ");
        dataValida = validarData(dataAtividade);
        if (!dataValida) {
            console.log("Data inválida. Por favor, digite novamente.");
        }
    } while (!dataValida);

    console.log("Descreva a atividade:");
    let descricaoAtividade = prompt(" ");

    console.log("Escolha uma categoria para a atividade:");
    for (let index = 0; index < categorias.length; index++) {
        console.log(`${index + 1}. ${categorias[index]}`);
    }
    let escolhaCategoria = parseInt(prompt("Digite o número da categoria: "));
    let categoriaAtividade = categorias[escolhaCategoria - 1];

    console.log("Digite a duração da atividade (em horas):");
    let duracaoAtividade = parseFloat(prompt(" "));

    console.log("Adicione uma nota a sua atividade:");
    let notaAtv = prompt(" ");

    atividades.push({ nome: nomeAtividade, data: dataAtividade, descricao: descricaoAtividade, categoria: categoriaAtividade, duracao: duracaoAtividade, nota: notaAtv });
    console.log("\nAtividade adicionada com sucesso!\n");
}

function visualizarElementos(elementos, titulo) {
    console.log(`${titulo} registrados(as):`);
    if (titulo === 'Atividades') {
        for (let index = 0; index < elementos.length; index++) {
            let elemento = elementos[index];
            console.log(`${index + 1}. Nome: ${elemento.nome} Data: ${elemento.data}, Descrição: ${elemento.descricao}, Categoria: ${elemento.categoria}, Duração: ${elemento.duracao} horas, Nota: ${elemento.nota}`);
        }
    } else {
        for (let index = 0; index < elementos.length; index++) {
            let elemento = elementos[index];
            console.log(`${index + 1}. ${elemento}`);
        }
    }
    console.log(" ");
}

function visualizarAtividades() {
    visualizarElementos(atividades, 'Atividades');
}

function visualizarAtividadesPorPeriodo() {
    let datainicio;
    let datafim;
    let dataValida = false;
    do {
        console.log("Digite a data de início (no formato 'yyyy-mm-dd'):");
        datainicio = prompt(" ");
        dataValida = validarData(datainicio);
        if (!dataValida) {
            console.log("Data inválida. Por favor, digite novamente.");
        }
    } while (!dataValida);

    dataValida = false;
    do {
        console.log("Digite a data de fim (no formato 'yyyy-mm-dd'):");
        datafim = prompt(" ");
        dataValida = validarData(datafim);
        if (!dataValida) {
            console.log("Data inválida. Por favor, digite novamente.");
        }
    } while (!dataValida);

    let atividadesFiltradas = [];
    for (let i = 0; i < atividades.length; i++) {
        let atividade = atividades[i];
        if (atividade.data >= datainicio && atividade.data <= datafim) {
            atividadesFiltradas.push(atividade);
        }
    }

    visualizarElementos(atividadesFiltradas, 'Atividades no período');
}

function relatorioAtividadesPorCategoria() {
    console.log("Relatório de Atividades por Categoria:");

    let relatorio = {};

    // Inicializa contadores para cada categoria
    for (let i = 0; i < categorias.length; i++) {
        const categoria = categorias[i];
        relatorio[categoria] = {
            totalTempo: 0,
            quantidadeAtividades: 0
        };
    }

    // Calcula o tempo total e quantidade de atividades por categoria
    for (let i = 0; i < atividades.length; i++) {
        const atividade = atividades[i];
        const categoria = atividade.categoria;

        if (relatorio[categoria]) {
            relatorio[categoria].totalTempo += atividade.duracao;
            relatorio[categoria].quantidadeAtividades++;
        }
    }

    // Exibe o relatório
    for (let i = 0; i < categorias.length; i++) {
        const categoria = categorias[i];
        console.log(`${categoria}:`);
        console.log(`  Total de tempo: ${relatorio[categoria].totalTempo} horas`);
        console.log(`  Quantidade de atividades: ${relatorio[categoria].quantidadeAtividades}`);
        console.log();
    }
}

function validarData(data) {
    if (data.length !== 10 || data[4] !== '-' || data[7] !== '-') {
        return false;
    }

    for (let i = 0; i < data.length; i++) {
        if (i !== 4 && i !== 7 && (data[i] < '0' || data[i] > '9')) {
            return false;
        }
    }

    const { ano, mes, dia } = extrairData(data);

    return ano > 0 && mes >= 1 && mes <= 12 && dia >= 1 && dia <= 31;
}

function extrairData(data) {
    const partes = data.split('-');
    const ano = parseInt(partes[0], 10);
    const mes = parseInt(partes[1], 10);
    const dia = parseInt(partes[2], 10);
    return { ano, mes, dia };
}

function mostrarMenu() {
    const opcoes = {
        1: adicionarAtividade,
        2: visualizarAtividades,
        3: visualizarAtividadesPorPeriodo,
        4: relatorioAtividadesPorCategoria,
        5: sair
    };

    let continuar = true;

    while (continuar) {
        console.log("Escolha uma opção:");
        console.log("1. Adicionar atividade");
        console.log("2. Visualizar atividades");
        console.log("3. Visualizar atividades por período");
        console.log("4. Relatório de atividades por categoria");
        console.log("5. Sair");

        let opcao = parseInt(prompt("Digite a opção desejada: "));
        console.log(" ");

        if (opcoes[opcao]) {
            opcoes[opcao]();
            if (opcao === 5) {
                continuar = false;
            }
        } else {
            console.log("Opção inválida. Escolha novamente.\n");
        }
    }
}

function sair() {
    console.log("Saindo do programa. Até mais!");
}

function iniciarDiario() {
    mostrarMenu();
}

iniciarDiario();
