
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>MarsTraders Pro</title>
    <link rel="icon" href="https://cdn-icons-png.flaticon.com/512/3075/3075775.png" />
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
      body { margin: 0; font-family: 'Segoe UI', sans-serif; }
      .bg-grid {
        background-image: radial-gradient(circle, rgba(60,60,80,0.1) 1px, transparent 0);
        background-size: 40px 40px;
        position: fixed;
        top: 0; left: 0; width: 100%; height: 100%;
        pointer-events: none;
        z-index: -1;
        opacity: 0.1;
      }
      @keyframes pulse { 0%, 100% { opacity: 0.6; } 50% { opacity: 1; } }
      .pulse { animation: pulse 2s infinite; }
    </style>
  </head>
  <body class="bg-black text-white">
    <div id="root"></div>
    <div class="bg-grid"></div>

    <script type="module">
      import React from 'https://esm.sh/react@18'
      import ReactDOM from 'https://esm.sh/react-dom@18/client'

      // 🎯 App Component
      function App() {
        const [countdown, setCountdown] = React.useState(6);
        const [lastTicks, setLastTicks] = React.useState([0, 0, 0, 0, 0, 0, 0, 0, 0, 0]);
        const [signal, setSignal] = React.useState({ dir: 'Rise', trades: 1, conf: 75 });
        const [balance, setBalance] = React.useState(1000);
        const [email, setEmail] = React.useState('');
        const [loggedIn, setLoggedIn] = React.useState(false);

        // Simulate new tick
        React.useEffect(() => {
          const interval = setInterval(() => {
            const newTick = Math.floor(Math.random() * 10);
            setLastTicks(prev => [newTick, ...prev.slice(0, 9)]);
            setCountdown(6);

            // Update signal
            setSignal({
              dir: ['Rise', 'Fall', 'Even', 'Odd'][Math.floor(Math.random() * 4)],
              trades: Math.floor(Math.random() * 3) + 1,
              conf: Math.floor(Math.random() * 21) + 75
            });

            // Simulate trade
            const profit = Math.random() > 0.5 ? 9.5 : -10;
            setBalance(prev => Math.round((prev + profit) * 100) / 100);
          }, 6000);

          return () => clearInterval(interval);
        }, []);

        const playSound = () => {
          const audio = new Audio("https://assets.mixkit.co/sfx/preview/mixkit-alarm-digital-clock-beep-989.mp3");
          audio.volume = 0.3;
          audio.play().catch(() => {});
        };

        const handleLogin = () => {
          if (email) {
            alert(`Login link sent to ${email}!`);
            setLoggedIn(true);
            playSound();
          }
        };

        if (!loggedIn) {
          return (
            <div className="min-h-screen flex items-center justify-center p-6">
              <div className="text-center bg-gray-900 p-8 rounded-2xl max-w-sm w-full shadow-2xl">
                <h1 className="text-3xl font-bold bg-gradient-to-r from-orange-400 to-red-600 bg-clip-text text-transparent mb-4">
                  🚀 MarsTraders Pro
                </h1>
                <p className="text-gray-300 mb-6">Enter your email to begin</p>
                <input
                  type="email"
                  placeholder="your@email.com"
                  value={email}
                  onChange={e => setEmail(e.target.value)}
                  className="w-full bg-gray-800 px-4 py-2 rounded-lg mb-4 border border-gray-600 focus:outline-none"
                />
                <button
                  onClick={handleLogin}
                  className="w-full bg-gradient-to-r from-blue-600 to-purple-700 py-2 rounded-lg font-medium hover:from-blue-700"
                >
                  🔐 Log In
                </button>
              </div>
            </div>
          );
        }

        return (
          <div className="min-h-screen">
            {/* Header */}
            <header className="bg-black/80 backdrop-blur-sm border-b border-gray-800 p-4 sticky top-0 z-10">
              <div className="max-w-4xl mx-auto flex justify-between items-center">
                <h1 className="text-2xl font-bold bg-gradient-to-r from-orange-400 to-red-600 bg-clip-text text-transparent">
                  MarsTraders Pro
                </h1>
                <button
                  onClick={() => setLoggedIn(false)}
                  className="text-xs text-gray-400 hover:text-white"
                >
                  Logout
                </button>
              </div>
            </header>

            <main className="max-w-4xl mx-auto p-6 space-y-6">
              {/* Status */}
              <div className="text-center">
                <div className="inline-flex items-center px-3 py-1 bg-green-900 text-green-100 rounded-full text-xs">
                  <span className="w-2 h-2 bg-green-400 rounded-full mr-2 pulse"></span>
                  Live Connection
                </div>
              </div>

              {/* Countdown */}
              <div className="text-center">
                <div className="inline-flex items-center justify-center w-24 h-24 rounded-full bg-gradient-to-br from-red-600 to-pink-700 text-white text-3xl font-bold shadow-lg">
                  {countdown}
                </div>
                <p className="mt-2 text-gray-400 text-sm">Seconds to Next Tick</p>
              </div>

              {/* Signal */}
              <div className="bg-gray-800 border border-gray-700 rounded-xl p-6">
                <h2 className="text-xl font-semibold mb-4">🤖 AI Signal</h2>
                <div className="flex flex-col sm:flex-row sm:items-center justify-between bg-gray-900 p-4 rounded-lg">
                  <span className="text-gray-300">Recommended: </span>
                  <span className="text-2xl font-bold text-blue-400">
                    Buy {signal.dir} × {signal.trades}
                  </span>
                </div>
                <p className="text-sm text-gray-400 mt-2">Confidence: {signal.conf}%</p>
              </div>

              {/* Balance */}
              <div className="text-center">
                <p className="text-sm text-gray-500">Virtual Balance</p>
                <p className={`text-2xl font-bold ${balance >= 1000 ? 'text-green-400' : 'text-red-400'}`}>
                  ${balance.toFixed(2)}
                </p>
              </div>

              {/* Last Ticks */}
              <div>
                <h3 className="text-lg font-medium mb-3">Last 10 Ticks</h3>
                <div className="flex gap-2 flex-wrap p-4 bg-gray-800 border border-gray-700 rounded-lg">
                  {lastTicks.map((t, i) => (
                    <span
                      key={i}
                      className={`w-10 h-10 flex items-center justify-center rounded-full font-bold text-sm ${
                        t % 2 === 0 ? 'bg-green-900 text-green-200' : 'bg-red-900 text-red-200'
                      }`}
                    >
                      {t}
                    </span>
                  ))}
                </div>
              </div>
            </main>

            <footer className="text-center text-gray-600 text-sm py-6">
              &copy; {new Date().getFullYear()} MarsTraders Pro
            </footer>
          </div>
        );
      }

      // Render App
      ReactDOM.createRoot(document.getElementById('root')).render(React.createElement(App));
    </script>
  </body>
</html>