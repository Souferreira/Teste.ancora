```
!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teste: Âncoras de Carreira</title>
    <link rel="stylesheet" href="styles.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        /* Estilos globais */
        body {
            font-family: Arial, sans-serif;
            margin: 0px;
            padding: 0px;
            background-color: #fdfdfe;
            color: #000000;
        }

        .container {
            max-width: 900px;
            margin: 15px auto;
            padding: 10px;
            background: linear-gradient(90deg, #eceded, #ced1d8, #aeb6c4);
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(184, 163, 163, 0.1);
        }

        header {
            background-color: #e1d6d6;
            color: #de5252;
            padding: 15px 0;
            text-align: center;
        }

        h1 {
            background: linear-gradient(90deg, #edeef0, #c0cbe1, #839ac5);
            border: 5px solid #8d7fd5;
            border-radius: 30px;
            padding: 15px;
            transition: background-color 0.3s ease, transform 0.3s ease;
            text-align: center;
            color: #100909;
        }
        h1:hover {
            background: linear-gradient(90deg, #88afb5, #9ec7d7, #26a5db);
            transform: scale(1.05);
        }
        p {
            background: linear-gradient(90deg, #edeef0, #c0cbe1, #839ac5);
            border: 5px solid #8d7fd5;
            border-radius: 30px;
            padding: 10px;
            transition: background-color 0.3s ease, transform 0.3s ease;
            text-align: center;
            color: #100909;
        }
        p:hover {
            background: linear-gradient(90deg, #88afb5, #9ec7d7, #26a5db);
            transform: scale(1.05);
        }
        table th, table td {
            border: 2px solid #e1d9d9;
            background: linear-gradient(90deg, #edeef0, #93afe6, #467cdf);
            padding: 1px;
            border-radius: 10px;
            text-align: center;
        }
        table th, table td:hover {
            background: linear-gradient(90deg, #88afb5, #9ec7d7, #26a5db);
            transform: scale(1.05);
        }
        /* Layout da lista de perguntas */
        .questions {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .question {
            background-color: #f7f7f7;
            border: 5px solid #8d7fd5;
            border-radius: 30px;
            padding: 15px;
            transition: background-color 0.3s ease, transform 0.3s ease;
            text-align: center;
        }

        .question:hover {
            background: linear-gradient(90deg, #88afb5, #9ec7d7, #26a5db);
            transform: scale(1.05);
        }

        .question input[type="radio"] {
            display: none;
        }

        .question label {
            display: inline-block;
            margin-right: 15px;
            cursor: pointer;
            text-align: center;
        }

        .circle {
            width: 40px;
            height: 40px;
            border-radius: 100%;
            background-color: #9b59b6;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            transition: background-color 0.3s;
        }

        .circle:hover {
            background-color: #ffbb33
        }

.question input[type="radio"]:checked + label .circle {
    background-color: #18b63a;
    color: #fff;
}

/* Estilos de formulário */
form {
    display: flex;
    flex-direction: column;
    gap: 30px;
    text-align: center;
}

button {
    padding: 10px 20px;
    background: linear-gradient(90deg, #c2aea6, #bf4444, #ed410c);
    color: rgb(248, 241, 241);
    border: none;
    border-radius: 15px;
    cursor: pointer;
    font-size: 13px;
    transition: background-color 0.3s, transform 0.3s;
}

button:hover {
    background: linear-gradient(90deg, #bbc8b8, #82d755, #3deb11);
    transform: scale(1.1);
}

.error-message {
    color: rgb(234, 11, 11);
    margin-top: 10px;
}

/* Responsividade */
@media (max-width: 768px) {
    .container {
        max-width: 100%;
        padding: 20px;
    }

    .questions {
        flex-direction: column;
        gap: 15px;
    }

    .question {
        padding: 10px;
    }

    .circle {
        width: 35px;
        height: 35px;
        font-size: 16px;
    }

    button {
        padding: 8px 16px;
        font-size: 18px;
    }

    table th, table td {
        font-size: 14px;
        padding: 5px;
    }
}

@media (max-width: 480px) {
    h1 {
        font-size: 24px;
    }

    .question {
        padding: 8px;
    }

    .circle {
        width: 30px;
        height: 30px;
        font-size: 14px;
    }

    button {
        padding: 6px 12px;
        font-size: 16px;
    }

    table th, table td {
        font-size: 12px;
        padding: 4px;
    }
}
</style>
</head>
<body>
<div class="container">
<h1>Teste: Âncoras de Carreira</h1>
<form id="career-anchors-form">
    <p>Leia atentamente as questões abaixo e clique na resposta que mais se aplica a você, em cada uma das afirmativas.</p>
    <div class="questions">
        <div class="question">
        </div>
    </div>
    <button type="button" id="calculate-btn">Ver resultado</button>
    <div class="error-message" id="error-message"></div>
    <button type="button" id="download-pdf" style="display:none;">Baixar PDF</button>
</form>
<div class="results">
    <canvas id="career-anchors-chart"></canvas>
</div>
</div>

<script>
 document.addEventListener('DOMContentLoaded', function() {
        const form = document.getElementById('career-anchors-form');
        const errorMessage = document.getElementById('error-message');
        const downloadButton = document.getElementById('download-pdf');
        const chartContainer = document.querySelector('.results');
        const chartCanvas = document.getElementById('career-anchors-chart');
        let answers = {};
    

const questions = [
    { question: "1. Sonho em ser tão bom no que faço que minha opinião de especialista seja sempre solicitada.", category: "COMPETÊNCIA TÉCNICA" },
    { question: "2. Me sinto mais realizado em meu trabalho quando sou capaz de integrar e gerenciar o trabalho dos outros.", category: "COMPETÊNCIA ADMINISTRATIVA" },
    
        ];

   
        // Gerar as questões
        function generateQuestions() {
            const questionsContainer = document.querySelector('.questions');
            questions.forEach((q, index) => {
                const questionElement = document.createElement('div');
                questionElement.classList.add('question');
                questionElement.innerHTML = `
                    <label for="question-${index}">
                        ${q.question}
                    </label>
                    <div class="options">
                    ${[1, 2, 3, 4, 5, 6, 7].map(value => `
                            <input type="radio" name="question-${index}" id="question-${index}-${value}" value="${value}" />
                            <label for="question-${index}-${value}">
                                <div class="circle">${value}</div>
                            </label>
                        `).join('')}
                    </div>
                `;
                questionsContainer.appendChild(questionElement);
            });
        }
    
        generateQuestions();
    
        // Calcular os resultados
        document.getElementById('calculate-btn').addEventListener('click', () => {
            let isValid = true;
            answers = {}; // Reset answers for each calculation
            questions.forEach((q, index) => {
                const selectedOption = form.querySelector(`input[name="question-${index}"]:checked`);
                if (!selectedOption) {
                    isValid = false;
                } else {
                    answers[q.category] = (answers[q.category] || 0) + parseInt(selectedOption.value);
                }
            });
    
            if (!isValid) {
                errorMessage.textContent = 'Por favor, responda todas as perguntas.';
                return;
            }
    
            errorMessage.textContent = '';
            generateChart();
            downloadButton.style.display = 'inline-block';
        });
    
        // Gerar gráfico
        function generateChart() {
    const categories = Object.keys(answers);
    const totalScore = Object.values(answers).reduce((a, b) => a + b, 0); // Soma total das pontuações
    const data = categories.map(category => answers[category]);
    const percentages = data.map(value => ((value / totalScore) * 100).toFixed(2)); // Cálculo em porcentagem
            
    new Chart(chartCanvas, {
        type: 'bar',
        data: {
            labels: categories,
            datasets: [{
                label: 'Pontuação (%) por categoria',
                data: percentages, // Usar porcentagens no gráfico
                backgroundColor: ['#ff6384', '#36a2eb', '#cc65fe', '#ffce56', '#ffbb33', '#2d8e3f', '#e745d3'],
                borderColor: '#ffffff',
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            plugins: {
                tooltip: {
                    callbacks: {
                        label: (context) => `${context.raw}%` // Exibir porcentagem no tooltip
                    }
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: 100 // Escala de 0 a 100%
                }
            }
        }
    });
}
    
        // Gerar PDF
      function generatePDF(scores) {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();

    // Título do PDF
    doc.setFontSize(16);
    doc.text("Teste: Âncoras de Carreira", 20, 20);

    // Detalhes do teste
    doc.setFontSize(12);
    let yPosition = 40;
    const totalScore = Object.values(scores).reduce((a, b) => a + b, 0); // Soma total das pontuações

    // Exibir pontuações e porcentagens
    Object.entries(scores).forEach(([category, score]) => {
        const percentage = ((score / totalScore) * 100).toFixed(2); // Cálculo em porcentagem
        doc.text(`${category}: ${score} (${percentage}%)`, 20, yPosition);
        yPosition += 10;
    });

    // Adicionar gráfico como imagem no PDF
    const chartImage = chartCanvas.toDataURL("image/png");
    doc.addImage(chartImage, 'PNG', 20, yPosition, 170, 100); // Adicionando imagem do gráfico

    yPosition += 110; // Ajustar posição para o texto descritivo

    // Adicionar explicações das âncoras de carreira
    const anchorsDescriptions = [
        " O que levar em consideração nas escolhas da sua carreira?",
        "Qual tipo de trabalho/emprego fará você levantar motivado(a) todos os dias para ir ao trabalho? Segundo Edgar Schein, especialista em desenvolvimento organizacional e ex-professor de Gestão da Sloan School Of Management- MIT,", 
        "as âncoras de carreira norteiam as nossas decisões profissionais e  demonstram realmente quais são os tipos de trabalhos/empregos que nos fará mais felizes.", 
        "As âncoras de carreira possibilitam um autoconhecimento, ficando mais fácil encontrar a melhor empresa para você trabalhar! Com base nas suas âncoras de carreira, você poderá perceber que tipos de empresa ou trabalho mais se adaptam aos seus valores, estilo de vida e predisposição para trilhar uma carreira.", 
        "Isso poderá eliminar desmotivação e descontentamento em escolhas inapropriadas.",
        "Quer descobrir quais são as suas âncoras de carreira?",
        "Autonomia e independência: Pessoas com essa âncora buscam maior independência e condições próprias de trabalho. Necessitam de poder de decisão e flexibilidade.",
        "Competência administrativa/geral: Indivíduos orientados para atingir altos níveis de responsabilidade na organização, focando em liderança e gestão geral.",
        "Competência técnica/funcional: Profissionais que se especializam em áreas específicas e buscam excelência técnica em seus campos de atuação.",
        "Criatividade empreendedora: Pessoas motivadas a criar ou reestruturar negócios próprios, com foco no empreendedorismo e na inovação.",
        "Dedicação a uma causa: Profissionais que direcionam sua carreira para contribuir com causas e valores em que acreditam, buscando impacto positivo no mundo.",
        "Desafio puro: Indivíduos motivados pela superação de desafios e solução de problemas complexos, com foco em conquistas excepcionais.",
        "Estilo de vida: Pessoas que buscam equilibrar trabalho, família e necessidades pessoais, priorizando flexibilidade e integração entre as áreas da vida.",
        "Segurança e estabilidade: Profissionais que valorizam previsibilidade, estabilidade financeira e segurança no ambiente de trabalho."
    ];

    anchorsDescriptions.forEach((description) => {
        if (yPosition >= 280) { // Verifica se há necessidade de uma nova página
            doc.addPage();
            yPosition = 20;
        }
        doc.text(description, 20, yPosition, { maxWidth: 170 });
        yPosition += 20;
    });

    // Salvar o PDF
    doc.save('resultado_ancoras_carreira.pdf');
}

// Adicionar evento ao botão de download
downloadButton.addEventListener("click", () => {
    generatePDF(answers); // Gerar e baixar o PDF com o resultado
});

    });
    
</script>  
</body>
</html>

```