import React, { useEffect, useMemo, useRef, useState } from 'react';
import {
  Heart,
  Camera,
  Gamepad2,
  NotebookPen,
  Settings,
  Image as Img,
  Download,
  Trash2,
  Sparkles,
  RotateCcw,
  Trophy,
  Search,
  Star,
} from 'lucide-react';
import './style.css';

type Page = 'home' | 'photo' | 'games' | 'journal' | 'settings';
type Photo = { id: string; src: string };
type J = {
  id: string;
  date: string;
  mood: string;
  title: string;
  entry: string;
  tags: string;
  moments: string;
  grateful: string;
};

type FilterKey = '' | 'vintage' | 'dreamy' | 'bw' | 'warm' | 'cool' | 'pink';

const FILTERS: Record<
  FilterKey,
  { label: string; css: string; canvas: string }
> = {
  '': {
    label: 'Natural',
    css: 'none',
    canvas: 'none',
  },
  vintage: {
    label: 'Vintage',
    css: 'sepia(0.55) contrast(1.05) saturate(0.8)',
    canvas: 'sepia(55%) contrast(105%) saturate(80%)',
  },
  dreamy: {
    label: 'Dreamy',
    css: 'brightness(1.12) contrast(0.88) saturate(0.85) blur(0.15px)',
    canvas: 'brightness(112%) contrast(88%) saturate(85%) blur(0.15px)',
  },
  bw: {
    label: 'B&W',
    css: 'grayscale(1) contrast(1.08)',
    canvas: 'grayscale(100%) contrast(108%)',
  },
  warm: {
    label: 'Warm',
    css: 'sepia(0.25) saturate(1.3) brightness(1.05)',
    canvas: 'sepia(25%) saturate(130%) brightness(105%)',
  },
  cool: {
    label: 'Cool',
    css: 'saturate(0.8) hue-rotate(12deg) brightness(1.03)',
    canvas: 'saturate(80%) hue-rotate(12deg) brightness(103%)',
  },
  pink: {
    label: 'Pink glow',
    css: 'saturate(1.25) hue-rotate(-10deg) brightness(1.05)',
    canvas: 'saturate(125%) hue-rotate(-10deg) brightness(105%)',
  },
};

const get = <T,>(k: string, d: T): T => {
  try {
    return JSON.parse(localStorage.getItem(k) || 'null') ?? d;
  } catch {
    return d;
  }
};

const put = (k: string, v: unknown) =>
  localStorage.setItem(k, JSON.stringify(v));

const td = () => new Date().toISOString().slice(0, 10);

function App() {
  const [p, setP] = useState<Page>('home');
  const [photos, setPhotos] = useState<Photo[]>(() =>
    get('fh.photos', []),
  );
  const [js, setJs] = useState<J[]>(() => get('fh.journal', []));
  const [scores, setScores] = useState(() =>
    get('fh.scores', { memory: 0, hearts: 0 }),
  );
  const [note, setNote] = useState(() =>
    get('fh.note', 'A tiny corner of the internet made just for you.'),
  );
  const [motion, setMotion] = useState(() => get('fh.motion', false));
  const [toast, setToast] = useState('');

  useEffect(() => put('fh.photos', photos), [photos]);
  useEffect(() => put('fh.journal', js), [js]);
  useEffect(() => put('fh.scores', scores), [scores]);
  useEffect(() => put('fh.note', note), [note]);
  useEffect(() => put('fh.motion', motion), [motion]);

  useEffect(() => {
    if (toast) {
      const t = setTimeout(() => setToast(''), 1800);
      return () => clearTimeout(t);
    }
  }, [toast]);

  return (
    <div className={motion ? 'app reduced' : 'app'}>
      <Float />

      <header>
        <button className="brand" onClick={() => setP('home')}>
          <span>
            <Heart fill="currentColor" />
          </span>
          <b>For Her</b>
          <small>a little place made with care</small>
        </button>

        <nav>
          {[
            ['home', 'Home'],
            ['photo', 'Photobooth'],
            ['games', 'Games'],
            ['journal', 'Journal'],
            ['settings', 'About'],
          ].map(([x, l]) => (
            <button
              key={x}
              className={p === x ? 'active' : ''}
              onClick={() => setP(x as Page)}
            >
              {l}
            </button>
          ))}
        </nav>
      </header>

      <main>
        {p === 'home' && (
          <Home
            go={setP}
            photos={photos}
            js={js}
            scores={scores}
          />
        )}

        {p === 'photo' && (
          <PhotoBooth
            photos={photos}
            setPhotos={setPhotos}
            toast={setToast}
          />
        )}

        {p === 'games' && (
          <Games scores={scores} setScores={setScores} />
        )}

        {p === 'journal' && (
          <Journal js={js} setJs={setJs} toast={setToast} />
        )}

        {p === 'settings' && (
          <SettingsPage
            note={note}
            setNote={setNote}
            motion={motion}
            setMotion={setMotion}
          />
        )}
      </main>

      {toast && <div className="toast">{toast}</div>}

      <footer>
        Everything stays in this browser · made for one very specific person.
      </footer>
    </div>
  );
}

function Float() {
  return (
    <div className="float">
      <Sparkles />
      <Star />
      <Heart />
    </div>
  );
}

function Home({
  go,
  photos,
  js,
  scores,
}: {
  go: (p: Page) => void;
  photos: Photo[];
  js: J[];
  scores: any;
}) {
  const j = js.find((x) => x.date === td());

  return (
    <section>
      <div className="hero">
        <div className="pill">
          <Sparkles size={14} /> private little corner of the internet
        </div>

        <h1>
          A little place
          <br />
          <em>made just for you.</em>
        </h1>

        <p>
          Take a few pictures, play something silly, or leave a little piece
          of your day here. No pressure, no audience — just a space that's
          yours.
        </p>

        <div className="actions">
          <button onClick={() => go('photo')}>Open photobooth</button>
          <button className="light" onClick={() => go('journal')}>
            Write today
          </button>
        </div>
      </div>

      <div className="dash">
        <Card icon={<NotebookPen />} title="Today">
          <b>{j?.mood || '—'}</b>
          <small>{j ? 'Entry saved' : 'No entry yet'}</small>
        </Card>

        <Card icon={<Img />} title="Little gallery">
          <b>{photos.length}</b>
          <small>memories saved</small>
        </Card>

        <Card icon={<Trophy />} title="Arcade">
          <b>{scores.hearts || 0}</b>
          <small>best hearts score</small>
        </Card>
      </div>

      <div className="features">
        <Feature
          go={() => go('photo')}
          icon={<Camera />}
          t="Photobooth"
          s="Filters, frames, strips & a local gallery."
        />

        <Feature
          go={() => go('games')}
          icon={<Gamepad2 />}
          t="Tiny arcade"
          s="Three little games for when you feel like playing."
        />

        <Feature
          go={() => go('journal')}
          icon={<NotebookPen />}
          t="Daily journal"
          s="Keep the good bits of ordinary days."
        />
      </div>
    </section>
  );
}

function Card({
  icon,
  title,
  children,
}: {
  icon: React.ReactNode;
  title: string;
  children: React.ReactNode;
}) {
  return (
    <div className="card">
      <span className="icon">{icon}</span>
      <b>{title}</b>
      <div>{children}</div>
    </div>
  );
}

function Feature({
  go,
  icon,
  t,
  s,
}: {
  go: () => void;
  icon: React.ReactNode;
  t: string;
  s: string;
}) {
  return (
    <button className="feature" onClick={go}>
      <span className="icon">{icon}</span>
      <b>{t}</b>
      <p>{s}</p>
    </button>
  );
}

function PhotoBooth({
  photos,
  setPhotos,
  toast,
}: {
  photos: Photo[];
  setPhotos: React.Dispatch<React.SetStateAction<Photo[]>>;
  toast: (x: string) => void;
}) {
  const video = useRef<HTMLVideoElement>(null);
  const canvas = useRef<HTMLCanvasElement>(null);
  const [on, setOn] = useState(false);
  const [demo, setDemo] = useState(false);
  const [filter, setFilter] = useState<FilterKey>('');
  const [shots, setShots] = useState(1);
  const [facing, setFacing] = useState<'user' | 'environment'>('user');
  const [count, setCount] = useState(0);
  const [strip, setStrip] = useState<string[]>([]);
  const [capturing, setCapturing] = useState(false);

  const stopCamera = () => {
    const stream = video.current?.srcObject as MediaStream | null;

    if (stream) {
      stream.getTracks().forEach((track) => track.stop());
    }

    if (video.current) {
      video.current.srcObject = null;
    }

    setOn(false);
  };

  useEffect(() => {
    return () => stopCamera();
  }, []);

  async function start() {
    try {
      if (!navigator.mediaDevices?.getUserMedia) {
        throw new Error('Camera API unavailable');
      }

      stopCamera();

      const s = await navigator.mediaDevices.getUserMedia({
        video: {
          facingMode: facing,
          width: { ideal: 1280 },
          height: { ideal: 960 },
        },
        audio: false,
      });

      if (video.current) {
        video.current.srcObject = s;
        await video.current.play();
      }

      setOn(true);
      setDemo(false);
    } catch {
      setDemo(true);
      toast('Camera unavailable — demo mode is ready.');
    }
  }

  async function flipCamera() {
    const nextFacing = facing === 'user' ? 'environment' : 'user';
    setFacing(nextFacing);

    if (on) {
      setTimeout(() => start(), 50);
    }
  }

  function snap() {
    if (capturing) return;

    setCapturing(true);
    setCount(3);

    let n = 3;

    const t = setInterval(() => {
      n--;
      setCount(n);

      if (!n) {
        clearInterval(t);
        capture();
      }
    }, 500);
  }

  function capture() {
    const currentCanvas = canvas.current;

    if (!currentCanvas) {
      setCapturing(false);
      return;
    }

    let src: string;

    if (on && video.current) {
      const width = video.current.videoWidth || 720;
      const height = video.current.videoHeight || 540;

      currentCanvas.width = width;
      currentCanvas.height = height;

      const x = currentCanvas.getContext('2d');

      if (!x) {
        setCapturing(false);
        return;
      }

      x.save();

      x.filter = FILTERS[filter].canvas;

      if (facing === 'user') {
        x.translate(width, 0);
        x.scale(-1, 1);
      }

      x.drawImage(video.current, 0, 0, width, height);
      x.restore();

      src = currentCanvas.toDataURL('image/jpeg', 0.9);
    } else {
      currentCanvas.width = 720;
      currentCanvas.height = 540;

      const x = currentCanvas.getContext('2d');

      if (!x) {
        setCapturing(false);
        return;
      }

      const g = x.createLinearGradient(0, 0, 720, 540);
      g.addColorStop(0, '#f7d8df');
      g.addColorStop(1, '#ddd2f0');

      x.fillStyle = g;
      x.fillRect(0, 0, 720, 540);

      x.fillStyle = '#6d5260';
      x.font = 'bold 42px system-ui';
      x.textAlign = 'center';
      x.fillText('your little moment', 360, 270);

      src = currentCanvas.toDataURL('image/jpeg', 0.9);
    }

    if (shots > 1) {
      const next = [...strip, src];

      if (next.length === shots) {
        setStrip([]);

        createPhotoStrip(next, filter)
          .then((stripSrc) => {
            save(stripSrc);
            toast(`${shots}-shot photo strip saved.`);
          })
          .catch(() => {
            next.forEach((item) => save(item));
            toast('Strip creation failed — individual photos were saved.');
          })
          .finally(() => setCapturing(false));
      } else {
        setStrip(next);
        setCapturing(false);
        toast(`Photo ${next.length} of ${shots} captured.`);
      }
    } else {
      save(src);
      setCapturing(false);
    }

    setCount(0);
  }

  function save(src: string) {
    setPhotos((x) => [
      {
        id: crypto.randomUUID(),
        src,
      },
      ...x,
    ]);

    toast('Saved to your little gallery.');
  }

  return (
    <section>
      <small className="label">PHOTOBOOTH</small>
      <h2>Make a little memory.</h2>

      <div className="photoGrid">
        <div className="panel">
          <div className="camera">
            <video
              ref={video}
              className="camera-video"
              style={{ filter: FILTERS[filter].css }}
              playsInline
              muted
            />

            {!on && (
              <div className="demo">
                {demo ? 'Demo mode' : 'Camera ready'}
                <small>
                  {demo
                    ? 'A sample scene keeps everything usable.'
                    : 'Tap start camera to begin.'}
                </small>
              </div>
            )}

            {count > 0 && <strong>{count}</strong>}

            <button
              className="capture"
              onClick={snap}
              disabled={capturing}
              aria-label="Take photo"
            >
              <Camera />
            </button>
          </div>

          <canvas ref={canvas} hidden />

          <div className="row">
            <button onClick={start}>
              {on ? 'Restart camera' : 'Start camera'}
            </button>

            <button className="light" onClick={flipCamera}>
              Flip
            </button>

            <button
              className="light"
              onClick={() => {
                stopCamera();
                setDemo(true);
              }}
            >
              Demo
            </button>
          </div>
        </div>

        <div className="side">
          <Control title="Style">
            <div className="chips">
              {(Object.entries(FILTERS) as [
                FilterKey,
                (typeof FILTERS)[FilterKey],
              ][]).map(([value, item]) => (
                <button
                  key={value || 'natural'}
                  className={filter === value ? 'sel' : ''}
                  onClick={() => setFilter(value)}
                >
                  {item.label}
                </button>
              ))}
            </div>
          </Control>

          <Control title="Photo strip">
            <div className="chips">
              {[1, 3, 4].map((n) => (
                <button
                  key={n}
                  className={shots === n ? 'sel' : ''}
                  onClick={() => {
                    setShots(n);
                    setStrip([]);
                  }}
                >
                  {n === 1 ? 'Single' : `${n} shots`}
                </button>
              ))}
            </div>
          </Control>

          {shots > 1 && (
            <div className="strip-status">
              <b>Strip progress</b>
              <span>
                {strip.length} / {shots} photos captured
              </span>
            </div>
          )}
        </div>
      </div>

      <div className="panel gallery">
        <div className="ghead">
          <div>
            <b>Little gallery</b>
            <small>{photos.length} saved locally</small>
          </div>
          <Img />
        </div>

        {!photos.length ? (
          <div className="empty">
            Your gallery is waiting for its first little memory.
          </div>
        ) : (
          <div className="galleryGrid">
            {photos.map((x) => (
              <div className="thumb" key={x.id}>
                <img src={x.src} alt="Saved memory" />

                <div>
                  <a
                    download={`for-her-${x.id}.jpg`}
                    href={x.src}
                    aria-label="Download photo"
                  >
                    <Download size={14} />
                  </a>

                  <button
                    onClick={() =>
                      setPhotos((p) =>
                        p.filter((y) => y.id !== x.id),
                      )
                    }
                    aria-label="Delete photo"
                  >
                    <Trash2 size={14} />
                  </button>
                </div>
              </div>
            ))}
          </div>
        )}
      </div>
    </section>
  );
}

async function createPhotoStrip(
  sources: string[],
  filter: FilterKey,
): Promise<string> {
  const images = await Promise.all(
    sources.map(
      (src) =>
        new Promise<HTMLImageElement>((resolve, reject) => {
          const image = new Image();

          image.onload = () => resolve(image);
          image.onerror = reject;
          image.src = src;
        }),
    ),
  );

  const width = 720;
  const padding = 28;
  const gap = 18;
  const footer = 95;
  const photoHeight = 500;

  const height =
    padding * 2 +
    footer +
    images.length * photoHeight +
    Math.max(0, images.length - 1) * gap;

  const output = document.createElement('canvas');
  output.width = width;
  output.height = height;

  const ctx = output.getContext('2d');

  if (!ctx) {
    throw new Error('Canvas unavailable');
  }

  ctx.fillStyle = '#fffaf7';
  ctx.fillRect(0, 0, width, height);

  let y = padding;

  images.forEach((image) => {
    const boxX = padding;
    const boxY = y;
    const boxW = width - padding * 2;
    const boxH = photoHeight;

    ctx.save();
    ctx.beginPath();
    ctx.roundRect(boxX, boxY, boxW, boxH, 18);
    ctx.clip();

    ctx.filter = FILTERS[filter].canvas;

    const imageRatio = image.width / image.height;
    const boxRatio = boxW / boxH;

    let drawWidth = boxW;
    let drawHeight = boxH;
    let drawX = boxX;
    let drawY = boxY;

    if (imageRatio > boxRatio) {
      drawHeight = boxH;
      drawWidth = boxH * imageRatio;
      drawX = boxX - (drawWidth - boxW) / 2;
    } else {
      drawWidth = boxW;
      drawHeight = boxW / imageRatio;
      drawY = boxY - (drawHeight - boxH) / 2;
    }

    ctx.drawImage(
      image,
      drawX,
      drawY,
      drawWidth,
      drawHeight,
    );

    ctx.restore();

    y += photoHeight + gap;
  });

  ctx.fillStyle = '#9d4660';
  ctx.textAlign = 'center';
  ctx.font = '600 25px system-ui, sans-serif';
  ctx.fillText('made with love · For Her', width / 2, height - 55);

  ctx.fillStyle = '#a48b96';
  ctx.font = '14px system-ui, sans-serif';
  ctx.fillText('a little memory to keep', width / 2, height - 30);

  return output.toDataURL('image/jpeg', 0.92);
}

function Control({
  title,
  children,
}: {
  title: string;
  children: React.ReactNode;
}) {
  return (
    <div className="control">
      <b>{title}</b>
      {children}
    </div>
  );
}

function Games({
  scores,
  setScores,
}: {
  scores: any;
  setScores: React.Dispatch<React.SetStateAction<any>>;
}) {
  const [g, setG] = useState<'memory' | 'ttt' | 'catch'>('memory');

  return (
    <section>
      <small className="label">GAMES</small>
      <h2>Tiny arcade, zero pressure.</h2>

      <div className="gameTabs">
        {[
          ['memory', 'Memory Match'],
          ['ttt', 'Tic-Tac-Toe'],
          ['catch', 'Catch the Hearts'],
        ].map(([v, l]) => (
          <button
            key={v}
            className={g === v ? 'sel' : ''}
            onClick={() => setG(v as 'memory' | 'ttt' | 'catch')}
          >
            {l}
            <small>
              Best{' '}
              {v === 'memory'
                ? scores.memory
                : v === 'catch'
                  ? scores.hearts
                  : scores.ttt || 0}
            </small>
          </button>
        ))}
      </div>

      <div className="panel game">
        {g === 'memory' ? (
          <Memory
            best={scores.memory}
            score={(n) =>
              setScores((s: any) => ({
                ...s,
                memory: Math.max(s.memory, n),
              }))
            }
          />
        ) : g === 'ttt' ? (
          <TTT />
        ) : (
          <Catch
            best={scores.hearts}
            score={(n) =>
              setScores((s: any) => ({
                ...s,
                hearts: Math.max(s.hearts, n),
              }))
            }
          />
        )}
      </div>
    </section>
  );
}

function Memory({
  best,
  score,
}: {
  best: number;
  score: (n: number) => void;
}) {
  const vals = ['🌷', '🍓', '🌙', '✨', '🫶', '☁️'];

  const make = () =>
    [...vals, ...vals]
      .sort(() => Math.random() - 0.5)
      .map((v, i) => ({
        i,
        v,
        o: false,
        d: false,
      }));

  const [c, setC] = useState(make);
  const [pick, setPick] = useState<number[]>([]);
  const [moves, setMoves] = useState(0);

  function tap(i: number) {
    if (pick.length === 2 || c[i].o || c[i].d) return;

    const n = [...pick, i];

    setC((a) =>
      a.map((x, j) =>
        j === i ? { ...x, o: true } : x,
      ),
    );

    setPick(n);

    if (n.length === 2) {
      setMoves((x) => x + 1);

      setTimeout(() => {
        const [a, b] = n;
        const ok = c[a].v === c[b].v;

        setC((z) =>
          z.map((x, j) =>
            j === a || j === b
              ? { ...x, o: ok, d: ok }
              : x,
          ),
        );

        setPick([]);

        if (ok) {
          const matched = c.filter((x, index) =>
            index === a || index === b ? true : x.d,
          ).length;

          if (matched === vals.length * 2) {
            score(moves + 1);
          }
        }
      }, 500);
    }
  }

  return (
    <div className="center">
      <h3>Memory Match</h3>
      <p>Match all six pairs.</p>

      <div className="memory">
        {c.map((x) => (
          <button key={x.i} onClick={() => tap(x.i)}>
            {x.o || x.d ? x.v : '·'}
          </button>
        ))}
      </div>

      <p>
        Moves {moves} · Best {best || '—'}
      </p>

      <button
        className="light"
        onClick={() => {
          setC(make());
          setPick([]);
          setMoves(0);
        }}
      >
        <RotateCcw size={14} /> New game
      </button>
    </div>
  );
}

function TTT() {
  const [b, setB] = useState<(string | null)[]>(
    Array(9).fill(null),
  );
  const [turn, setTurn] = useState('X');
  const w = winner(b);

  function play(i: number) {
    if (b[i] || w) return;

    const n = [...b];
    n[i] = turn;
    setB(n);

    if (!winner(n)) {
      setTurn(turn === 'X' ? 'O' : 'X');
    }
  }

  return (
    <div className="center">
      <h3>Tic-Tac-Toe</h3>
      <p>You are X. Tap a square.</p>

      <div className="ttt">
        {b.map((x, i) => (
          <button key={i} onClick={() => play(i)}>
            {x}
          </button>
        ))}
      </div>

      <p>
        {w
          ? `${w} wins!`
          : b.every(Boolean)
            ? 'Draw.'
            : `Turn: ${turn}`}
      </p>

      <button
        className="light"
        onClick={() => {
          setB(Array(9).fill(null));
          setTurn('X');
        }}
      >
        Reset
      </button>
    </div>
  );
}

function winner(b: (string | null)[]) {
  for (const [a, c, d] of [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ] as number[][]) {
    if (b[a] && b[a] === b[c] && b[a] === b[d]) {
      return b[a];
    }
  }

  return null;
}

function Catch({
  best,
  score,
}: {
  best: number;
  score: (n: number) => void;
}) {
  const [r, setR] = useState(false);
  const [s, setS] = useState(0);
  const [hs, setHs] = useState<any[]>([]);

  useEffect(() => {
    if (!r) return;

    const a = setInterval(
      () =>
        setHs((x) => [
          ...x,
          {
            id: Math.random(),
            x: Math.random() * 90,
            y: Math.random() * 80,
          },
        ]),
      450,
    );

    const b = setTimeout(() => {
      setR(false);
      setHs([]);
      score(s);
    }, 15000);

    return () => {
      clearInterval(a);
      clearTimeout(b);
    };
  }, [r]);

  return (
    <div className="center">
      <h3>Catch the Hearts</h3>
      <p>15 seconds. Catch as many as you can.</p>

      <div className="catch">
        {!r && (
          <button
            onClick={() => {
              setS(0);
              setR(true);
            }}
          >
            Start game
          </button>
        )}

        {hs.map((h) => (
          <button
            key={h.id}
            style={{
              left: `${h.x}%`,
              top: `${h.y}%`,
            }}
            onClick={() => {
              setS((x) => x + 1);
              setHs((x) =>
                x.filter((y) => y.id !== h.id),
              );
            }}
          >
            ♥
          </button>
        ))}
      </div>

      <p>
        Score {s} · Best {best}
      </p>
    </div>
  );
}

function Journal({
  js,
  setJs,
  toast,
}: {
  js: J[];
  setJs: React.Dispatch<React.SetStateAction<J[]>>;
  toast: (x: string) => void;
}) {
  const [d, setD] = useState(td());

  const j =
    js.find((x) => x.date === d) || {
      id: '',
      date: d,
      mood: '✨',
      title: '',
      entry: '',
      tags: '',
      moments: '',
      grateful: '',
    };

  const [search, setSearch] = useState('');
  const [form, setForm] = useState(j);

  useEffect(() => {
    setForm(
      js.find((x) => x.date === d) || {
        id: '',
        date: d,
        mood: '✨',
        title: '',
        entry: '',
        tags: '',
        moments: '',
        grateful: '',
      },
    );
  }, [d, js]);

  useEffect(() => {
    const t = setTimeout(() => {
      if (
        form.title ||
        form.entry ||
        form.tags ||
        form.moments ||
        form.grateful
      ) {
        const x = {
          ...form,
          id: form.id || crypto.randomUUID(),
        };

        setJs((a) => [
          x,
          ...a.filter((z) => z.date !== d),
        ]);

        toast('Journal autosaved.');
      }
    }, 700);

    return () => clearTimeout(t);
  }, [form]);

  const list = useMemo(
    () =>
      js.filter((x) =>
        (x.title + x.entry + x.tags + x.date)
          .toLowerCase()
          .includes(search.toLowerCase()),
      ),
    [js, search],
  );

  return (
    <section>
      <small className="label">JOURNAL</small>
      <h2>Keep the good bits.</h2>

      <div className="journalGrid">
        <div className="panel editor">
          <div className="row">
            <input
              type="date"
              value={d}
              onChange={(e) => setD(e.target.value)}
            />

            <button
              className="light"
              onClick={() =>
                setForm({
                  ...form,
                  mood: form.mood === '🌷' ? '✨' : '🌷',
                })
              }
            >
              {form.mood} Mood
            </button>
          </div>

          <input
            className="title"
            value={form.title}
            onChange={(e) =>
              setForm({
                ...form,
                title: e.target.value,
              })
            }
            placeholder="Give the day a tiny title…"
          />

          <div className="moods">
            {['✨', '🌷', '☀️', '🌙', '🫶', '🍓'].map(
              (m) => (
                <button
                  key={m}
                  className={form.mood === m ? 'sel' : ''}
                  onClick={() =>
                    setForm({
                      ...form,
                      mood: m,
                    })
                  }
                >
                  {m}
                </button>
              ),
            )}
          </div>

          <textarea
            className="entry"
            value={form.entry}
            onChange={(e) =>
              setForm({
                ...form,
                entry: e.target.value,
              })
            }
            placeholder="What happened? What made you smile? Write without editing yourself…"
          />

          <Field
            l="Tags"
            v={form.tags}
            set={(v) =>
              setForm({
                ...form,
                tags: v,
              })
            }
          />

          <Field
            l="Favorite moments"
            v={form.moments}
            set={(v) =>
              setForm({
                ...form,
                moments: v,
              })
            }
          />

          <Field
            l="Today I'm grateful for"
            v={form.grateful}
            set={(v) =>
              setForm({
                ...form,
                grateful: v,
              })
            }
          />
        </div>

        <aside>
          <div className="panel search">
            <Search size={16} />
            <input
              value={search}
              onChange={(e) => setSearch(e.target.value)}
              placeholder="Search your days…"
            />
          </div>

          <div className="panel">
            <b>Previous days</b>

            {list.length === 0 ? (
              <p>
                No entries yet. This space is yours to fill slowly.
              </p>
            ) : (
              list.slice(0, 15).map((x) => (
                <div className="entryRow" key={x.id}>
                  <button onClick={() => setD(x.date)}>
                    {x.mood}
                    <span>
                      {x.title || 'Untitled day'}
                      <small>{x.date}</small>
                    </span>
                  </button>

                  <button
                    onClick={() => {
                      setJs((a) =>
                        a.filter((y) => y.id !== x.id),
                      );
                      toast('Entry deleted.');
                    }}
                  >
                    <Trash2 size={14} />
                  </button>
                </div>
              ))
            )}
          </div>
        </aside>
      </div>
    </section>
  );
}

function Field({
  l,
  v,
  set,
}: {
  l: string;
  v: string;
  set: (x: string) => void;
}) {
  return (
    <label className="field">
      <small>{l}</small>
      <textarea
        value={v}
        onChange={(e) => set(e.target.value)}
        placeholder="Write a little…"
      />
    </label>
  );
}

function SettingsPage({
  note,
  setNote,
  motion,
  setMotion,
}: {
  note: string;
  setNote: (x: string) => void;
  motion: boolean;
  setMotion: (x: boolean) => void;
}) {
  return (
    <section className="settings">
      <small className="label">ABOUT</small>
      <h2>A few little settings.</h2>

      <div className="panel">
        <b>Your little note</b>
        <p>Edit the message that lives inside this place.</p>
        <textarea
          value={note}
          onChange={(e) => setNote(e.target.value)}
        />
      </div>

      <div className="panel settingRow">
        <div>
          <b>Reduced motion</b>
          <p>Tone down animations and floating details.</p>
        </div>

        <button
          className={motion ? 'toggle on' : 'toggle'}
          onClick={() => setMotion(!motion)}
        >
          <i />
        </button>
      </div>

      <div className="panel">
        <b>About this app</b>
        <p>
          Photos, journal entries, scores and this note stay in
          localStorage. No account, backend or external API is
          required.
        </p>
      </div>
    </section>
  );
}

createRoot(document.getElementById('root')!).render(<App />);
