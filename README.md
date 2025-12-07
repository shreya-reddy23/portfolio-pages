<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Linear Algebra Portfolio — Shreya M.</title>
  <meta name="description" content="Portfolio on Linear Algebra applications in Engineering (Automation & Robotics)">
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--muted:#9aa4b2;--accent:#3b82f6;--glass:rgba(255,255,255,0.03)}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;background:linear-gradient(180deg,#071028 0%, #081824 100%);color:#e6eef8;line-height:1.5}
    .wrap{max-width:1000px;margin:36px auto;padding:24px}
    header{display:flex;gap:16px;align-items:center}
    .avatar{width:84px;height:84px;border-radius:12px;background:linear-gradient(135deg,#2563eb,#7c3aed);display:flex;align-items:center;justify-content:center;font-weight:700;font-size:28px;color:white}
    h1{margin:0;font-size:28px}
    .tag{color:var(--muted);font-size:14px}
    nav{margin-top:18px;display:flex;gap:10px;flex-wrap:wrap}
    a.btn{background:var(--glass);padding:10px 14px;border-radius:10px;color:#cfe6ff;text-decoration:none;font-size:14px;transition:transform .15s ease}
    a.btn:hover{transform:translateY(-4px)}
    .hero{margin-top:22px;padding:18px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.03)}
    .intro p{margin:8px 0;color:var(--muted)}
    .grid{display:grid;grid-template-columns:1fr;gap:18px;margin-top:18px}
    .card{background:var(--card);padding:18px;border-radius:12px;border:1px solid rgba(255,255,255,0.03)}
    h2{margin:0 0 10px 0}
    pre{background:rgba(0,0,0,0.25);padding:12px;border-radius:8px;overflow:auto;color:#dbeafe}
    ol,ul{padding-left:20px}
    footer{margin-top:26px;color:var(--muted);font-size:13px}
    .example{background:linear-gradient(90deg, rgba(59,130,246,0.06), rgba(124,58,237,0.03));padding:12px;border-radius:8px;border:1px solid rgba(255,255,255,0.02)}
    .toc{display:flex;gap:10px;flex-wrap:wrap}
    @media(min-width:880px){.grid{grid-template-columns:1fr 1fr}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="avatar">SR</div>
      <div>
        <h1>Linear Algebra for Engineering — Portfolio</h1>
        <div class="tag">Shreya M. • 3rd Semester — Automation & Robotics</div>
      </div>
    </header>

    <nav class="toc">
      <a class="btn" href="#intro">Introduction</a>
      <a class="btn" href="#ch1">Chapter 1: Why Linear Algebra in Robotics</a>
      <a class="btn" href="#ch2">Chapter 2: Gaussian Elimination & LU</a>
      <a class="btn" href="#ch3">Chapter 3: Vector Spaces & Linear Transform</a>
      <a class="btn" href="#references">References</a>
    </nav>

    <section class="hero" id="intro">
      <div class="intro">
        <p><strong>Introduction (short)</strong></p>
        <p>Linear algebra studies vectors, matrices and linear mappings — the language used to model and compute multi-dimensional data. In engineering, and especially in robotics, linear algebra provides concise tools for representing motion, forces, sensor data and control laws. This portfolio explains key topics and gives practical robotics examples.</p>
      </div>
    </section>

    <main class="grid">
      <!-- CHAPTER 1 -->
      <article class="card" id="ch1">
        <h2>Chapter 1 — Why Linear Algebra is Required in Robotics</h2>
        <p>Robots operate in 2D/3D spaces and handle multiple signals at once. Linear algebra is used for:</p>
        <ul>
          <li>Representing positions and orientations with vectors and matrices (homogeneous transforms).</li>
          <li>Solving kinematics and dynamics: mapping joint velocities to end-effector velocities using Jacobians (matrix derivatives).</li>
          <li>State estimation and sensor fusion (Kalman filters use linear models and matrices).</li>
        </ul>

        <div class="example">
          <strong>Example 1 — Homogeneous transform (2D):</strong>
          <p>To represent a 2D pose (x, y, θ) we use a 3×3 homogeneous matrix:</p>
          <pre>H = [[cosθ, -sinθ, x],
 [sinθ,  cosθ, y],
 [   0,     0,  1]]</pre>
          <p>Multiply H by a homogeneous point [px, py, 1]^T to get the point in world coordinates.</p>
        </div>

        <div style="margin-top:10px">
          <strong>Example 2 — Jacobian (2-link planar arm)</strong>
          <p>For a 2-link arm with lengths l1,l2 and joint angles θ1,θ2, the Jacobian maps joint velocities to end-effector velocity v = J(θ)·θ̇. Computing J uses partial derivatives and yields a 2×2 matrix.</p>
          <pre>// pseudocode
x = l1*cos(t1) + l2*cos(t1+t2)
y = l1*sin(t1) + l2*sin(t1+t2)
J = [[-l1*sin(t1)-l2*sin(t1+t2), -l2*sin(t1+t2)],
     [ l1*cos(t1)+l2*cos(t1+t2),  l2*cos(t1+t2)]]
</pre>
        </div>
      </article>

      <!-- CHAPTER 2 -->
      <article class="card" id="ch2">
        <h2>Chapter 2 — Gaussian Elimination &amp; LU Decomposition (in Robotics)</h2>
        <p>Many robotics problems reduce to solving linear systems A·x = b (e.g., constraints, least-squares linearized steps, calibration). Gaussian elimination and LU decomposition are fundamental methods to solve these efficiently.</p>

        <div class="example">
          <strong>Short explanation</strong>
          <p>Gaussian elimination transforms A to an upper-triangular matrix U and then performs back-substitution. LU factorizes A into L (lower) and U (upper) so multiple right-hand sides b can be solved quickly using forward/back substitution.</p>
        </div>

        <div style="margin-top:10px">
          <strong>Example — Solving for joint torques (toy):</strong>
          <p>Suppose a simplified static linear model K·τ = F where K is a stiffness matrix, τ are joint torques and F is external force vector. To find τ we solve Kτ = F.</p>
          <pre>// Using MATLAB/Octave
K = [50, -10; -10, 30];
F = [12; 8];
% Solve with Gaussian elimination (built-in):
tau = K \ F;
% LU (explicit):
[L,U,P] = lu(K); % P for pivoting
y = L \ (P*F);
tau2 = U \ y;
</pre>
          <p>Both tau and tau2 should match. LU is faster when solving for many F vectors with same K (e.g., time stepping).</p>
        </div>

        <div style="margin-top:10px">
          <strong>When to prefer LU vs direct elimination</strong>
          <ul>
            <li>Use LU if you will solve with the same A and many different b's.</li>
            <li>Use Gaussian elimination (direct solve) for one-off solves or small matrices.</li>
          </ul>
        </div>
      </article>

      <!-- CHAPTER 3 -->
      <article class="card" id="ch3">
        <h2>Chapter 3 — Vector Spaces &amp; Linear Transformations</h2>
        <p>Vector spaces define the algebraic rules for vectors (addition, scalar multiplication). Linear transformations are functions T satisfying T(u+v)=T(u)+T(v) and T(αu)=αT(u). In robotics, these represent coordinate frame changes, projections, and sensor models.</p>

        <div class="example">
          <strong>Example — Change of basis</strong>
          <p>If you have a vector expressed in the robot base frame and want coordinates in tool frame, you apply a transformation matrix. The matrix representation depends on basis vectors.</p>
          <pre>// Simple change-of-basis in MATLAB
B = [b1 b2 b3]; % columns are basis vectors in world coords
v_world = [1;2;3];
coords_in_B = B \ v_world; % coordinates of v in basis B
</pre>
        </div>

        <div style="margin-top:10px">
          <strong>Example — Projection (used in computer vision)</strong>
          <p>Projection matrices map 3D points into image coordinates (linear after homogeneous division). These are 3x4 matrices built from camera intrinsics and extrinsics.</p>
        </div>

      </article>

      <section class="card" id="references">
        <h2>References & Next steps</h2>
        <ul>
          <li>Books: "Linear Algebra and Its Applications" by Gilbert Strang (clear intuition and engineering examples).</li>
          <li>Robotics: "Introduction to Robotics: Mechanics and Control" by John J. Craig (for kinematics & Jacobians).</li>
          <li>Practice: implement examples in MATLAB / Python (NumPy) and include simple animations or plots for your portfolio.</li>
        </ul>
        <p style="margin-top:10px">Want: I can add interactive plots, small animations (SVG/CSS), or export this as a GitHub Pages-ready index.html. Tell me which extra feature you want next.</p>
      </section>

    <section class="card" id="anim1">
  <h2>Interactive Animation — 2‑Link Robot Arm</h2>
  <p>This simple JavaScript animation shows how a 2‑link planar arm moves when you change joint angles θ1 and θ2.</p>
  <div style="background:#0002;padding:12px;border-radius:8px;margin-bottom:10px">
    <canvas id="armCanvas" width="400" height="300" style="background:#111;border-radius:8px"></canvas>
  </div>
  <pre>
// JS included inside the same HTML file
&lt;script&gt;
const canvas = document.getElementById("armCanvas");
const ctx = canvas.getContext("2d");
let t1 = 30*Math.PI/180;
let t2 = 45*Math.PI/180;
const l1=100, l2=80;
function drawArm(){
  ctx.clearRect(0,0,400,300);
  ctx.strokeStyle="#4fc3f7";
  ctx.lineWidth=4;
  const x1 = 200 + l1*Math.cos(t1);
  const y1 = 150 + l1*Math.sin(t1);
  const x2 = x1 + l2*Math.cos(t1+t2);
  const y2 = y1 + l2*Math.sin(t1+t2);
  ctx.beginPath(); ctx.moveTo(200,150); ctx.lineTo(x1,y1); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(x1,y1); ctx.lineTo(x2,y2); ctx.stroke();
}
drawArm();
&lt;/script&gt;
  </pre>
</section>


<section class="card" id="interactivePlot">
  <h2>Interactive Plot — 2D Linear Transformation Visualizer</h2>
  <p>This tool shows how a 2×2 matrix transforms a set of points. Adjust matrix values and see the transformation instantly.</p>
  <div style="background:#0002;padding:12px;border-radius:8px;margin-bottom:10px">
    <canvas id="plotCanvas" width="400" height="300" style="background:#111;border-radius:8px"></canvas>
  </div>
  <pre>
// JavaScript linear transformation plot
tconst pCanvas = document.getElementById("plotCanvas");
const pCtx = pCanvas.getContext("2d");
let A = [[1,0],[0,1]]; // identity transform
function drawPlot(){
  pCtx.clearRect(0,0,400,300);
  pCtx.fillStyle="#4fc3f7";
  for(let x=-5; x<=5; x++){
    for(let y=-5; y<=5; y++){
      const nx = A[0][0]*x + A[0][1]*y;
      const ny = A[1][0]*x + A[1][1]*y;
      pCtx.fillRect(200+nx*10,150-ny*10,2,2);
    }
  }
}
drawPlot();
  </pre>
</section>

<!-- GitHub Pages Ready Note -->
<section class="card" id="pagesReady">
  <h2>GitHub Pages Ready</h2>
  <p>This file is fully self-contained (HTML + CSS + JS). Just upload it as <code>index.html</code> to a GitHub repository and enable GitHub Pages. No extra setup needed.</p>
</section>

</main>

    <footer>
      <p>Tip: save this file as <code>index.html</code> and push it to a GitHub repository. Enable Pages to host the portfolio. Good luck — Shreya!</p>
    </footer>
  </div>
</body>
</html>

