// Variável global para armazenar a leitura atual
let leituraAtual = null; 
let estaLendo = false;

function lerTexto(texto) {
    // 1. Garante que uma nova leitura substitua a anterior corretamente
    if (window.speechSynthesis) {
        window.speechSynthesis.cancel(); 
    }

    // 2. Se já estiver lendo, impede que múltiplas leituras rodem juntas
    if (estaLendo) {
        return; 
    }

    estaLendo = true;
    
    // Configura a nova leitura
    leituraAtual = new SpeechSynthesisUtterance(texto);
    
    // Libera a trava assim que a leitura terminar ou falhar
    leituraAtual.onend = () => { estaLendo = false; };
    leituraAtual.onerror = () => { estaLendo = false; };

    // Executa a leitura
    window.speechSynthesis.speak(leituraAtual);
}

// 3. Proteção contra cliques rápidos e repetidos no botão
document.querySelectorAll('.botao-ler').forEach(botao => {
    botao.addEventListener('click', (evento) => {
        const cartao = evento.target.closest('.cartao');
        const textoParaLer = cartao.querySelector('.cartao-frente p').innerText;
        
        // Executa a função com estabilidade
        lerTexto(textoParaLer);
    });
});
