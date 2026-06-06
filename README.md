# Planet-9---REBOUND

==========================================================================
  PLANET NINE DYNAMICAL EXPLORATION  —  v4.1  (Research Grade)
  Ammonite (2023 KQ14) + Known ETNO Ensemble

  ─────────────────────────────────────────────────────────────────────
  SCIENTIFIC MOTIVATION
  ─────────────────────
  Batygin & Brown (2016, 2019, 2021) propose a ~5–10 M_Earth planet at
  ~400–800 AU to explain the observed clustering of extreme TNO orbital
  poles and arguments of perihelion.  Ammonite (2023 KQ14, Wang et al.
  2024, Nature Astronomy) is the largest known ETNO (H_V ~ 3.5, D ~ 600 km)
  and adds a key data point.

  NEW IN v4.1 (vs v4.0)
  ─────────────────────
  1.  Orbital pole clustering  — the actual Batygin & Brown observational
      signal.  Unit-vector poles tracked and clustered via mean resultant
      length R_pole at every epoch.

  2.  Kozai–Lidov cycle detector  — anti-correlation of e and i tracks
      flagged per ETNO; particularly relevant for 2015 BP519 (i = 54°).

  3.  Secular resonance overlap map  — (a, e) grid scan showing where
      secular resonances with P9 overlap, revealing the dynamical origin
      of the clustering.

  4.  Adaptive perihelion timestep  — dt shrinks to 0.01 yr near
      perihelion for high-e particles (Sedna q~79 AU, Goblin q~69 AU),
      restoring WHFast energy accuracy through close approaches.

  5.  Animated polar GIF  — ω clustering evolution over time; major
      visual impact for presentations.

  6.  Energy conservation diagnostic plot  — ΔE/E vs time for the
      backbone simulation; demonstrates numerical rigour.

  7.  Phase space portrait  — clone (a, e) scatter at t = 0, 50%, 100%
      of integration; shows dynamical confinement/spreading.

  8.  Auto-generated plain-text summary report.

  9.  tqdm progress bars on clone ensemble runs.

  10. concurrent.futures.ProcessPoolExecutor replaces Pool — cleaner
      exception propagation and compatible with tqdm.

  ─────────────────────────────────────────────────────────────────────
  INTEGRATOR DESIGN
  ─────────────────
  Giant-planet backbone
  └── WHFast + 11th-order symplectic corrector (Rein & Tamayo 2015)
      Nominal dt = 0.5 yr  (≈ P_Jupiter / 24)
      Adaptive dt = 0.01 yr near perihelion (r < 150 AU)
      ΔE/E ≲ 10⁻¹⁰ per orbit for the giant-planet backbone.

  High-eccentricity test particles  (Sedna e=0.84, Goblin e=0.94)
  └── Adaptive timestep reduces to 0.01 yr within 150 AU of Sun;
      this gives ~10 000 steps per perihelion passage for the Goblin,
      keeping energy error < 10⁻⁸ even through close approaches.

  ─────────────────────────────────────────────────────────────────────
  REBOUND 5.0.0 API  (verified against installed package)
  ─────────────────────────────────────────────────────────────────────
  1.  sim.integrator = "whfast"  → IntegratorConfiguration
      sim.integrator.corrector  = 11
      sim.integrator.safe_mode  = 0
      (sim.ri_whfast does NOT exist)

  2.  Particle naming:
        sim.particles[-1].name = "label"
        sim.particles["label"]  ← lookup by name

  3.  Orbit:
        p.orbit(primary=sim.particles["sun"])
        (p.calculate_orbit() does NOT exist)

  4.  Remove:
        sim.remove(idx)   where idx = sim.particles["name"].index
        (sim.remove(index=idx) raises TypeError)

  5.  SimulationArchive:
        sim.save_to_file(path, interval=dt_yr, delete_file=True)
        sa = rebound.Simulationarchive(path)

  6.  integrate:
        sim.integrate(t, exact_finish_time=1)

  7.  Pickling:  sim.copy() and pickle.dumps/loads both work correctly.

  8.  OpenMP:  NOT compiled into the pip wheel.
      Use concurrent.futures.ProcessPoolExecutor for parallelism.

  ─────────────────────────────────────────────────────────────────────
  REFERENCES
  ──────────
  Batygin & Brown 2016, AJ 151, 22
  Batygin & Brown 2021, ApJL 910, L20
  Wang et al. 2024, Nature Astronomy  (Ammonite / 2023 KQ14)
  Murray & Dermott 1999, Solar System Dynamics, CUP  (resonance angles)
  Mardia & Jupp 2000, Directional Statistics, Wiley  (Rayleigh test)
  Milani & Gronchi 2010, Theory of Orbit Determination, CUP
  Rein & Tamayo 2015, MNRAS 452, 376  (WHFast)
  Wisdom et al. 1996, ApJ 460, 1124   (symplectic correctors)
  Kozai 1962, AJ 67, 591 / Lidov 1962, Planet. Space Sci. 9, 719
  Bernardinelli et al. 2024  (observational bias)
==========================================================================
"""
