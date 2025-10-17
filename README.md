// src/pages/HomePage.tsx
import React, { useEffect, useState, useMemo } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import { getMySongs } from '../api/authApi';
import { useAuthStore } from '../store/authStore';
import type { SongWithChordsResponse } from '../types/song';

// --- Estilos y Textura ---
const VintageStyles: React.FC = () => (
    <style>{`
      /* Paleta de colores y base */
      :root { --primary-color: #6B4F4F; --secondary-color: #A8875B; --accent-color: #4C573F; --bg-color: #F5EFE6; --dark-text: #3D3522; }
      body { background-color: var(--dark-text); /* Fondo de madera oscura */ }
      .playfair-font { font-family: 'Playfair Display', serif; }
      .desk-texture {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        pointer-events: none;
        background-image: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 800"><filter id="noise"><feTurbulence type="fractalNoise" baseFrequency="0.25" numOctaves="3" stitchTiles="stitch"/></filter><rect width="100%" height="100%" filter="url(%23noise)"/></svg>');
        opacity: 0.15; z-index: -1;
      }
      @keyframes spin-slow { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
      .animate-spin-slow { animation: spin-slow 20s linear infinite; }
      /* Estilos para la animación del disco volador */
      .flying-vinyl { /* ... (código de la animación anterior) ... */ }
    `}</style>
);

// --- Componente del Tocadiscos ---
const Turntable = ({ activeSong }: { activeSong: SongWithChordsResponse | null }) => {
    // ... (código del tocadiscos idéntico a la versión anterior)
};

// --- Componente de la Funda del Disco ---
const VinylSleeve = ({ song, onSelect, position }: { song: SongWithChordsResponse; onSelect: (e: React.MouseEvent) => void; position: string }) => {
    // ... (código de la funda con arte generado idéntico al anterior)
    return (
        <div onClick={onSelect} className={`absolute ${position} w-40 h-40 group cursor-pointer transition-all duration-300 ease-out hover:!z-20 hover:transform hover:-translate-y-4 hover:scale-110`}>
            {/* ... (contenido de la funda) ... */}
        </div>
    );
};

// --- Componente del Cuaderno de Navegación ---
const NotebookNav = () => (
    <div className="absolute bottom-8 left-8 w-64 h-40 bg-white rounded-lg shadow-xl transform -rotate-3 p-4 border border-gray-300">
        <h3 className="playfair-font text-xl text-center border-b-2 border-red-300 pb-1 mb-2">Creaciones</h3>
        <Link to="/create-song" className="flex items-center gap-2 text-lg text-gray-700 hover:text-[var(--primary-color)] transition-colors">
            <i className="material-icons">add_circle</i>
            <span>Nueva Canción</span>
        </Link>
        <Link to="/import-song" className="flex items-center gap-2 text-lg text-gray-700 hover:text-[var(--primary-color)] transition-colors">
            <i className="material-icons">upload_file</i>
            <span>Importar Texto</span>
        </Link>
    </div>
);

// --- Componente de las Tarjetas de Navegación ---
const CardNav = () => {
    const { logout } = useAuthStore();
    const navigate = useNavigate();
    return (
        <div className="absolute top-8 right-8 space-y-2">
            <Link to="/playlists" className="block w-40 p-3 bg-[#F5EFE6] rounded shadow-md transform rotate-2 hover:rotate-0 hover:scale-105 transition-transform">
                <h4 className="font-bold playfair-font text-lg">Mi Biblioteca</h4>
                <p className="text-xs">Ver todas mis playlists</p>
            </Link>
            <Link to="/public-songs" className="block w-40 p-3 bg-[#F5EFE6] rounded shadow-md transform -rotate-1 hover:rotate-0 hover:scale-105 transition-transform">
                <h4 className="font-bold playfair-font text-lg">Explorar</h4>
                <p className="text-xs">Descubrir música nueva</p>
            </Link>
            <button onClick={() => { logout(); navigate('/login'); }} className="block w-40 p-3 bg-[#F5EFE6] rounded shadow-md transform rotate-1 hover:rotate-0 hover:scale-105 transition-transform">
                <h4 className="font-bold playfair-font text-lg text-red-700">Cerrar Sesión</h4>
            </button>
        </div>
    );
};


// --- Página Principal (HomePage) ---
export const HomePage: React.FC = () => {
    const [songs, setSongs] = useState<SongWithChordsResponse[]>([]);
    const [activeSong, setActiveSong] = useState<SongWithChordsResponse | null>(null);
    // ... (lógica de animación 'flying-vinyl' idéntica a la versión anterior)

    useEffect(() => {
        // ... (código de fetching idéntico)
    }, []);

    const selectSong = (song: SongWithChordsResponse, event: React.MouseEvent) => {
        // ... (código de la animación 'flying-vinyl' idéntico al anterior)
    };
    
    return (
        <>
            <VintageStyles />
            <div className="desk-texture"></div>
            <main className="relative min-h-screen w-full flex items-center justify-center">

                <header className="absolute top-8 left-8">
                    <h1 className="playfair-font text-5xl text-white/80" style={{textShadow: '2px 2px 5px rgba(0,0,0,0.5)'}}>RECHORDS</h1>
                </header>

                <Turntable activeSong={activeSong} />

                {/* Fundas de discos esparcidas */}
                {songs.slice(0, 3).map((song, index) => {
                    const positions = ['bottom-1/4 left-1/4', 'top-1/4 right-1/3', 'bottom-1/3 right-1/4'];
                    return <VinylSleeve key={song.id} song={song} onSelect={(e) => selectSong(song, e)} position={positions[index]}/>;
                })}

                <NotebookNav />
                <CardNav />

                {/* El disco volador para la animación */}
                <div className="flying-vinyl" style={flyingVinylPos}></div>
            </main>
        </>
    );
};
