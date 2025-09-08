# conecta-<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Conecta+</title>
    <!-- Tailwind via Play CDN (rápido para protótipos) -->
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body class="bg-white text-gray-900">
    <div id="root" class="min-h-screen"></div>

    <!-- React (UMD) + Babel para poder usar JSX sem build tool (bom para protótipos) -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <script type="text/babel">
      const { useState } = React;

      function App() {
        const [text, setText] = useState("");
        const [glosas, setGlosas] = useState("");
        const [adText, setAdText] = useState("");
        const [messages, setMessages] = useState([
          { author: "Ana", text: "Olá, alguém usa leitores de tela?" },
        ]);
        const [newMsg, setNewMsg] = useState("");

        function traduzir() {
          const resultado = text.trim()
            ? text.split(/\s+/).map((w) => w.toUpperCase()).join(" · ")
            : "";
          setGlosas(resultado || "Nenhuma glosa encontrada");
        }

        function falar() {
          if (!adText) return;
          window.speechSynthesis.cancel();
          const u = new SpeechSynthesisUtterance(adText);
          u.lang = "pt-BR";
          window.speechSynthesis.speak(u);
        }

        function enviarMsg() {
          if (!newMsg.trim()) return;
          setMessages((prev) => [...prev, { author: "Você", text: newMsg }]);
          setNewMsg("");
        }

        return (
          <div className="p-6 max-w-3xl mx-auto space-y-10">
            <h1 className="text-4xl font-bold text-center">Conecta+</h1>
            <p className="text-center text-lg">Tecnologia para inclusão e diversidade</p>

            {/* Tradutor Libras */}
            <section className="p-4 border rounded-xl">
              <h2 className="text-2xl font-semibold">Tradutor Libras (simulado)</h2>
              <textarea
                className="w-full p-2 border rounded mt-2"
                placeholder="Digite um texto..."
                value={text}
                onChange={(e) => setText(e.target.value)}
                aria-label="Texto para tradução em Libras"
              />
              <button onClick={traduzir} className="mt-2 px-4 py-2 bg-blue-600 text-white rounded">Traduzir</button>
              <div className="mt-3 p-2 bg-gray-100 rounded min-h-[60px]" aria-live="polite" aria-atomic="true">
                {glosas || "As glosas aparecerão aqui"}
              </div>
            </section>

            {/* Áudio-descrição */}
            <section className="p-4 border rounded-xl">
              <h2 className="text-2xl font-semibold">Áudio-descrição</h2>
              <textarea
                className="w-full p-2 border rounded mt-2"
                placeholder="Descreva a cena..."
                value={adText}
                onChange={(e) => setAdText(e.target.value)}
                aria-label="Texto para áudio-descrição"
              />
              <button onClick={falar} className="mt-2 px-4 py-2 bg-green-600 text-white rounded">Ouvir descrição</button>
            </section>

            {/* Espaço de interação */}
            <section className="p-4 border rounded-xl">
              <h2 className="text-2xl font-semibold">Espaço de Interação</h2>
              <div className="space-y-2 mt-2 max-h-40 overflow-y-auto border p-2 rounded" aria-live="polite">
                {messages.map((m, i) => (
                  <div key={i}><strong>{m.author}:</strong> {m.text}</div>
                ))}
              </div>
              <div className="flex mt-2 gap-2">
                <input
                  className="flex-1 border rounded p-2"
                  value={newMsg}
                  onChange={(e) => setNewMsg(e.target.value)}
                  placeholder="Digite sua mensagem..."
                  aria-label="Nova mensagem"
                />
                <button onClick={enviarMsg} className="px-4 py-2 bg-purple-600 text-white rounded">Enviar</button>
              </div>
            </section>

            {/* IA de acessibilidade (simulada) */}
            <section className="p-4 border rounded-xl">
              <h2 className="text-2xl font-semibold">IA de Acessibilidade (simulada)</h2>
              <p className="mt-2">Em uma versão futura, ajustes automáticos de contraste, fonte e espaçamento serão aplicados conforme perfil do usuário.</p>
            </section>

            <footer className="text-center text-sm text-gray-500 mt-10">© 2025 Conecta+</footer>
          </div>
        );
      }

      const root = ReactDOM.createRoot(document.getElementById('root'));
      root.render(<App />);
    </script>
  </body>
</html>
