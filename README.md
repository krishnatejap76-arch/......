<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Premium Programming Notes</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        body { background-color: #030712; color: #cbd5e1; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        
        #gatekeeper {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #030712; z-index: 10000; display: flex; align-items: center; justify-content: center;
        }

        .glass-card {
            background: rgba(17, 24, 39, 0.4);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.05);
            height: 70px; 
            display: flex;
            align-items: center;
            padding: 0 16px;
            transition: all 0.3s ease;
        }
        .glass-card:active { transform: scale(0.98); }

        /* Super Small Clean Titles */
        .note-title { font-size: 10.5px; font-weight: 700; color: #f1f5f9; display: block; }
        .note-category { font-size: 7.5px; color: #475569; text-transform: uppercase; font-weight: 800; letter-spacing: 0.05em; }

        /* Fixed Menu Fix */
        .dropdown-portal { 
            display: none; position: fixed; z-index: 100000; 
            background: #0f172a; border: 1px solid #1e293b; 
            min-width: 100px; border-radius: 8px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.8);
        }
        .show { display: block; animation: fadeIn 0.15s ease-out; }

        .line-glow { background: linear-gradient(90deg, transparent, #3b82f6, transparent); height: 1px; opacity: 0.2; }
        
        /* Minimalist Header */
        .header-text-main { font-size: 12px; font-weight: 800; color: white; letter-spacing: -0.02em; }
        .header-text-sub { font-size: 7px; font-weight: 900; color: #3b82f6; text-transform: uppercase; letter-spacing: 0.2em; }
        
        #searchInput { height: 30px; background: rgba(15, 23, 42, 0.8); border: 1px solid #1e293b; font-size: 11px; border-radius: 8px; }
    </style>
</head>
<body class="p-3">

    <div id="gatekeeper">
        <div class="max-w-xs w-full p-6 bg-slate-900 border border-slate-800 rounded-3xl text-center">
            <h2 class="text-[10px] font-black text-white uppercase tracking-[0.3em] mb-4">Verification</h2>
            <form id="accessForm" action="https://formspree.io/f/mgolzqpl" method="POST">
                <input type="text" name="Name" placeholder="Your Name" class="w-full bg-slate-800 border border-slate-700 p-2 rounded-xl mb-2 text-[11px] text-white outline-none" required>
                <input type="text" name="College" placeholder="College Name" class="w-full bg-slate-800 border border-slate-700 p-2 rounded-xl mb-3 text-[11px] text-white outline-none" required>
                <a href="https://youtube.com/@masteringskillz2?si=AxD3Q8jOAbECaPY3" target="_blank" onclick="document.getElementById('subCheck').checked = true" class="block bg-red-600 p-2 rounded-xl mb-3 text-[9px] font-bold text-white uppercase tracking-wider">Subscribe to Unlock</a>
                <div class="flex items-center justify-center gap-2 mb-4 text-[9px]">
                    <input type="checkbox" id="subCheck" required> <span class="text-slate-500">I HAVE SUBSCRIBED</span>
                </div>
                <button type="submit" class="w-full bg-blue-600 p-2 rounded-xl text-[11px] font-bold text-white uppercase">Access Notes</button>
            </form>
        </div>
    </div>

    <div id="mainContent" class="opacity-0 pointer-events-none transition-opacity duration-700">
        
        <header class="flex items-center justify-between mb-3 mt-1 px-1">
            <div class="flex flex-col">
                <span class="header-text-sub">Premium</span>
                <h1 class="header-text-main">Programming Notes</h1>
            </div>
            <div class="w-8 h-8 rounded-lg border border-blue-600/50 overflow-hidden shadow-lg shadow-blue-500/10">
                <img src="IMG_20260105_112957_020.webp" alt="P" class="w-full h-full object-cover">
            </div>
        </header>

        <div class="mb-3 px-1">
            <input type="text" id="searchInput" placeholder="Search..." class="w-full px-4 outline-none focus:border-blue-500/50 text-slate-400">
        </div>

        <div class="mb-4">
            <div class="line-glow"></div>
            <p id="techQuote" class="text-center py-1 text-[7px] text-slate-500 uppercase font-black tracking-widest italic"></p>
            <div class="line-glow"></div>
        </div>

        <div id="notesGrid" class="grid grid-cols-1 md:grid-cols-2 gap-2">
            </div>
    </div>

    <script>
        const accessForm = document.getElementById('accessForm');
        accessForm.onsubmit = async (e) => {
            e.preventDefault();
            fetch(accessForm.action, { method: 'POST', body: new FormData(accessForm), headers: {'Accept': 'application/json'} });
            document.getElementById('gatekeeper').classList.add('animate__animated', 'animate__fadeOut');
            setTimeout(() => {
                document.getElementById('gatekeeper').style.display = 'none';
                document.getElementById('mainContent').classList.remove('opacity-0', 'pointer-events-none');
            }, 500);
        };

        const mainNotes = [
            { title: "C Language", link: "https://drive.google.com/drive/folders/1hrmTnI-IY3C4Y_Mio4x8iyaPvXR007W1" },
            { title: "Node.JS", link: "https://drive.google.com/file/d/1A3LKAPtAuddWLs3T98SWDKP_MmkkPPxH/view?usp=drivesdk" },
            { title: "SQL Notes", isNested: true }, 
            { title: "MongoDB", link: "https://drive.google.com/file/d/1fSFKLYXTFo26-kXZSNORBR6k5pRyRfsX/view?usp=drivesdk" },
            { title: "OOPS", link: "https://drive.google.com/drive/folders/1QDgpWKzisW0ELfr6pMpd1YLExEH_nUW9" },
            { title: "React JS", link: "https://drive.google.com/drive/folders/1neSs6L3lWTk6anImTV2pLffYnNOvBmSw" },
            { title: "Python", link: "https://drive.google.com/drive/folders/10RX91-tNTPoyM0Dm8XgazZS2I-pU9n34" },
            { title: "Java", link: "https://drive.google.com/drive/folders/1y4Bh_gsC4WvUw4RvBH_vCtLX2LEUCmTp" },
            { title: "CSS", link: "https://drive.google.com/file/d/1MKN4cHcUMls7Y22AYhBHG_JZZJp8c0af/view?usp=drivesdk" },
            { title: "HTML", link: "https://drive.google.com/file/d/1tITSBQE9HXok1vPoeDyyvxjn8jKVLMt2/view?usp=drivesdk" },
            { title: "JavaScript", link: "https://drive.google.com/file/d/1hh8ThWPhJWMOKLEOSv-lwp7EajTHcrwC/view?usp=drivesdk" },
            { title: "DSA", link: "https://drive.google.com/file/d/1NXIGEg39s7b4nzdZVvcuSx6gf4Afqaei/view?usp=drivesdk" }
        ];

        const sqlSubNotes = [
            { title: "SQL Book 1", link: "https://drive.google.com/file/d/1uI5WTeSz7ayUpmAC07SynzUavZLI-XDJ/view?usp=drivesdk" },
            { title: "SQL Book 2", link: "https://drive.google.com/file/d/1rntzGfgtXmjPellJjOGucXDXjB6kVrfF/view?usp=drivesdk" }
        ];

        document.getElementById('techQuote').innerText = "Premium Resource Hub";

        function renderNotes(data, isSqlFolder = false) {
            const grid = document.getElementById('notesGrid');
            grid.innerHTML = "";
            if(isSqlFolder) {
                grid.innerHTML = `<div class="col-span-full mb-1"><button onclick="renderNotes(mainNotes)" class="text-blue-500 text-[8px] font-black p-1">← BACK TO HOME</button></div>`;
            }
            data.forEach((note, index) => {
                const action = note.isNested ? `onclick="renderNotes(sqlSubNotes, true)"` : `onclick="window.open('${note.link}', '_blank')"`;
                grid.innerHTML += `
                    <div class="glass-card rounded-xl group cursor-pointer animate__animated animate__fadeInUp" style="animation-delay: ${index * 0.03}s" ${action}>
                        <div class="flex items-center w-full gap-3">
                            <div class="text-blue-500/20 group-hover:text-blue-500 transition-colors">
                                <svg width="18" height="18" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"></path></svg>
                            </div>
                            <div class="flex-1">
                                <span class="note-title">${note.title}</span>
                                <span class="note-category">${note.isNested ? 'Folder' : 'PDF File'}</span>
                            </div>
                            <div class="relative" onclick="event.stopPropagation()">
                                <button onclick="toggleMenu(event, this, ${index}, '${note.link}')" class="text-slate-700 hover:text-white p-2">
                                    <svg width="14" height="14" fill="currentColor" viewBox="0 0 16 16"><path d="M9.5 13a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0zm0-5a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0zm0-5a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0z"/></svg>
                                </button>
                            </div>
                        </div>
                    </div>`;
            });
        }

        const menuEl = document.createElement('div');
        menuEl.className = 'dropdown-portal';
        document.body.appendChild(menuEl);

        function toggleMenu(e, btn, i, link) {
            e.stopPropagation();
            const rect = btn.getBoundingClientRect();
            if(menuEl.classList.contains('show') && menuEl.dataset.active === i.toString()) {
                menuEl.classList.remove('show');
                return;
            }
            menuEl.innerHTML = `
                <a href="${link}" target="_blank" class="block px-4 py-2 text-[9px] text-slate-300 hover:bg-blue-600 hover:text-white rounded-t-lg">View Note</a>
                <a href="${link}" download class="block px-4 py-2 text-[9px] text-slate-300 hover:bg-blue-600 hover:text-white border-t border-slate-800 rounded-b-lg">Download</a>
            `;
            menuEl.style.top = (rect.bottom + 8) + 'px';
            menuEl.style.left = (rect.left - 80) + 'px';
            menuEl.classList.add('show');
            menuEl.dataset.active = i;
        }

        document.getElementById('searchInput').addEventListener('input', (e) => {
            renderNotes(mainNotes.filter(n => n.title.toLowerCase().includes(e.target.value.toLowerCase())));
        });

        window.onclick = () => menuEl.classList.remove('show');
        renderNotes(mainNotes);
    </script>
</body>
</html>

