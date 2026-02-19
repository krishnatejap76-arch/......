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

        /* Glass Card with Hover Lift */
        .glass-card {
            background: rgba(17, 24, 39, 0.4);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.05);
            height: 75px; 
            display: flex;
            align-items: center;
            padding: 0 16px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .glass-card:hover {
            background: rgba(30, 41, 59, 0.6);
            border-color: #3b82f6;
            transform: translateY(-2px);
            box-shadow: 0 10px 20px -10px rgba(59, 130, 246, 0.3);
        }

        .note-title { font-size: 11px; font-weight: 700; color: #f1f5f9; display: block; line-height: 1.2; }
        .note-category { font-size: 8px; color: #475569; text-transform: uppercase; font-weight: 800; margin-top: 2px; }

        /* Floating Menu */
        .dropdown-portal { 
            display: none; position: fixed; z-index: 999999; 
            background: #0f172a; border: 1px solid #1e293b; 
            min-width: 120px; border-radius: 12px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.7);
            animation: menuFade 0.2s ease;
        }
        @keyframes menuFade { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
        .show { display: block; }

        /* Profile Glow Animation */
        .profile-container {
            width: 38px; height: 38px; border-radius: 10px; 
            border: 2px solid #3b82f6; overflow: hidden;
            flex-shrink: 0;
            animation: pulse-blue 3s infinite;
        }
        @keyframes pulse-blue { 0% { box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.4); } 70% { box-shadow: 0 0 0 10px rgba(59, 130, 246, 0); } 100% { box-shadow: 0 0 0 0 rgba(59, 130, 246, 0); } }

        .line-glow { background: linear-gradient(90deg, transparent, #3b82f6, transparent); height: 1px; opacity: 0.3; }
        #searchInput { height: 36px; background: rgba(15, 23, 42, 0.9); border: 1px solid #1e293b; font-size: 12px; border-radius: 12px; }
    </style>
</head>
<body class="p-3">

    <div id="gatekeeper">
        <div class="max-w-xs w-full p-6 bg-slate-900 border border-slate-800 rounded-3xl text-center animate__animated animate__fadeIn">
            <p class="text-[10px] font-black text-blue-500 uppercase tracking-widest mb-4">Verification</p>
            <form id="accessForm" action="https://formspree.io/f/mgolzqpl" method="POST">
                <input type="text" name="Name" placeholder="Name" class="w-full bg-slate-800 border border-slate-700 p-2.5 rounded-xl mb-3 text-xs text-white" required>
                <input type="text" name="College" placeholder="College" class="w-full bg-slate-800 border border-slate-700 p-2.5 rounded-xl mb-3 text-xs text-white" required>
                <a href="https://youtube.com/@masteringskillz2?si=AxD3Q8jOAbECaPY3" target="_blank" onclick="document.getElementById('subCheck').checked = true" class="block bg-red-600 p-2.5 rounded-xl mb-3 text-[10px] font-bold text-white transition active:scale-95">SUBSCRIBE CHANNEL</a>
                <div class="flex items-center justify-center gap-2 mb-4 text-[10px]">
                    <input type="checkbox" id="subCheck" required> <span>I HAVE SUBSCRIBED</span>
                </div>
                <button type="submit" class="w-full bg-blue-600 p-2.5 rounded-xl font-bold text-xs text-white">UNLOCK NOW</button>
            </form>
        </div>
    </div>

    <div id="mainContent" style="opacity: 0; pointer-events: none;">
        
        <header class="flex items-center justify-between mb-4 mt-2 animate__animated animate__slideInDown">
            <div class="flex flex-col">
                <span class="text-[8px] font-black text-blue-500 uppercase tracking-[0.2em]">Premium</span>
                <h1 class="text-[14px] font-bold text-white">Programming Notes</h1>
            </div>
            
            <div class="profile-container">
                <img src="IMG_20260105_112957_020.webp" alt="P" class="w-full h-full object-cover">
            </div>
        </header>

        <div class="mb-4 animate__animated animate__fadeIn animate__delay-1s">
            <input type="text" id="searchInput" placeholder="Search language..." class="w-full px-5 outline-none focus:border-blue-500 text-slate-300">
        </div>

        <div class="mb-6 animate__animated animate__fadeIn animate__delay-1s">
            <div class="line-glow"></div>
            <p id="techQuote" class="text-center py-2 text-[8px] text-slate-500 uppercase font-black italic tracking-widest"></p>
            <div class="line-glow"></div>
        </div>

        <div id="notesGrid" class="grid grid-cols-1 md:grid-cols-2 gap-3">
            </div>
    </div>

    <script>
        const accessForm = document.getElementById('accessForm');
        accessForm.onsubmit = async (e) => {
            e.preventDefault();
            fetch(accessForm.action, { method: 'POST', body: new FormData(accessForm), headers: { 'Accept': 'application/json' } });
            document.getElementById('gatekeeper').classList.add('animate__animated', 'animate__fadeOut');
            setTimeout(() => {
                document.getElementById('gatekeeper').style.display = 'none';
                document.getElementById('mainContent').style.cssText = "opacity: 1; pointer-events: auto; transition: 0.8s ease;";
                renderNotes(mainNotes); // Trigger staggered animation on load
            }, 600);
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

        document.getElementById('techQuote').innerText = "Premium Handwritten Notes";

        function renderNotes(data, isSqlFolder = false) {
            const grid = document.getElementById('notesGrid');
            grid.innerHTML = "";
            if(isSqlFolder) {
                grid.innerHTML = `<div class="col-span-full mb-1"><button onclick="renderNotes(mainNotes)" class="text-blue-500 text-[9px] font-bold p-1">← BACK TO LIST</button></div>`;
            }
            data.forEach((note, index) => {
                const action = note.isNested ? `onclick="renderNotes(sqlSubNotes, true)"` : `onclick="window.open('${note.link}', '_blank')"`;
                grid.innerHTML += `
                    <div class="glass-card rounded-2xl group cursor-pointer animate__animated animate__fadeInUp" style="animation-delay: ${index * 0.07}s" ${action}>
                        <div class="flex items-center w-full gap-4">
                            <div class="text-blue-500/30 group-hover:text-blue-400 transition-colors">
                                <svg width="22" height="22" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"><path d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"></path></svg>
                            </div>
                            <div class="flex-1">
                                <span class="note-title">${note.title}</span>
                                <span class="note-category">${note.isNested ? 'Folder' : 'PDF Study Material'}</span>
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
                <a href="${link}" target="_blank" class="block px-4 py-3 text-[11px] text-slate-300 hover:bg-blue-600 hover:text-white border-b border-slate-800 rounded-t-xl">View Note</a>
                <a href="${link}" download class="block px-4 py-3 text-[11px] text-slate-300 hover:bg-blue-600 hover:text-white rounded-b-xl">Download PDF</a>
            `;
            menuEl.style.top = (rect.bottom + 10) + 'px';
            menuEl.style.left = (rect.left - 90) + 'px';
            menuEl.classList.add('show');
            menuEl.dataset.active = i;
        }

        document.getElementById('searchInput').addEventListener('input', (e) => {
            renderNotes(mainNotes.filter(n => n.title.toLowerCase().includes(e.target.value.toLowerCase())));
        });

        window.onclick = () => menuEl.classList.remove('show');
    </script>
</body>
</html>
