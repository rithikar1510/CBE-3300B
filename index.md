<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>CBE3300B: Humidity Chamber Project Hub</title>

  <style>
    /* Reset & Base */
    body {
      margin: 0;
      padding: 0;
      display: flex;
      background: #f0f4f8;
      font-family: Georgia, serif;
      color: #001f3f;
    }

    /* Left Navigation */
    nav.toc {
      position: fixed;
      top: 0;
      left: 0;
      width: 220px;
      height: 100vh;
      background: #001f3f;
      padding: 1em;
      box-shadow: 2px 0 4px rgba(0,0,0,0.1);
      overflow-y: auto;
    }

    nav.toc ul {
      list-style: none;
      margin: 0;
      padding: 0;
    }

    nav.toc li {
      margin: 0.5em 0;
    }

    nav.toc a {
      display: block;
      padding: 0.5em 1em;
      color: white;
      text-decoration: none;
      font-weight: 600;
      border-radius: 4px;
      transition: background 0.2s;
    }

    nav.toc a:hover {
      background: #003366;
    }

    /* Main Content */
    main {
      flex: 1;
      margin-left: 200px;
      display: flex;
      justify-content: center;
      padding: 2em;
    }

    .container {
      width: 95%;
      max-width: 6400px;
      background: white;
      padding: 2em;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    /* Typography */
    h1 {
      text-align: center;
      font-size: 2.2em;
      color: #001f3f;
      margin-bottom: 0.5em;
    }

    h2 {
      font-size: 1.75em;
      margin-top: 1.5em;
      border-bottom: 2px solid #ccc;
      padding-bottom: 0.2em;
      color: #001f3f;
    }

    h3 {
      font-size: 1.3em;
      margin-top: 1em;
      color: #001f3f;
    }

    p, ul {
      line-height: 1.6;
      margin-bottom: 1em;
    }

    ul {
      padding-left: 1.5em;
    }

    a {
      color: #001f3f;
    }

    a:hover {
      text-decoration: underline;
    }

    .button-link {
      display: inline-block;
      background: #001f3f;
      color: white;
      padding: 0.5em 1em;
      margin: 0.25em;
      border-radius: 4px;
      text-decoration: none;
    }

    .button-link:hover {
      background: #003366;
      color: white;
    }

    iframe {
      width: 100%;
      height: 500px;
      border: 1px solid #ccc;
      margin-top: 1em;
    }

    footer {
      text-align: center;
      font-size: 0.9em;
      color: #666;
      margin-top: 2em;
      border-top: 1px solid #ddd;
      padding-top: 1em;
    }
  </style>
</head>

<body>

  <!-- Sidebar Navigation -->
  <nav class="toc">
    <ul>
      <li><a href="#about">1 About</a></li>
      <li><a href="#reports">2 Project Reports</a></li>
      <li><a href="#gantt">3 Project Timeline</a></li>
    </ul>
  </nav>

  <!-- Main Content -->
  <main>
    <div class="container">

      <!-- Header -->
      <header>
        <h1>CBE3300B:<br>Humidity Chamber Project Hub</h1>
        <p style="text-align:center; font-size:1.2em;">
          University of Pennsylvania<br>
          Chemical and Biomolecular Engineering<br>
          Spring 2025
        </p>
      </header>

      <!-- About -->
      <section id="about">
        <h2>1&nbsp;About</h2>
        <p>
          This repository serves as the primary documentation hub for our CBE3300B humidity chamber project.
          Our objective is to design, prototype, and refine a controlled humidity chamber system capable of
          maintaining stable environmental conditions for testing and experimentation.
        </p>
        <p>
          This site consolidates all major project milestones, design reports, prototype updates, and scheduling resources
          into a unified platform for collaboration and reproducibility.
        </p>
      </section>

      <!-- Reports -->
      <section id="reports">
        <h2>2&nbsp;Project Reports & Deliverables</h2>
        <p>
          Access each stage of our project development below:
        </p>

        <div style="text-align:center;">
          <a href="preliminary-design-report.html" class="button-link">Preliminary Design Report</a>
          <a href="initial-design-report.html" class="button-link">Initial Design Report</a>
          <a href="initial-prototype.html" class="button-link">Initial Prototype</a>
          <a href="intermediate-prototype.html" class="button-link">Intermediate Prototype</a>
          <a href="min-viable-prod.html" class="button-link">Minimum Viable Product</a>
          <a href="phys-device-design.html" class="button-link">Physical Device Design</a>
          <a href="cbe-principles.html" class="button-link">Chemical Engineering Principles</a>
          <a href="future-directions.html" class="button-link">Future Directions</a>
        </div>
      </section>

      <!-- Gantt -->
      <section id="gantt">
        <h2>3&nbsp;Project Timeline</h2>

        <p>
          Our full project schedule is maintained via a GANTT chart to track development,
          testing, and milestone deadlines.
        </p>

        <p style="text-align:center;">
          <a href="https://docs.google.com/spreadsheets/d/12-z6ueIstVCXI8U_jozlMdH24LCAA0FeEc4j1kiNmzk/edit?usp=sharing"
             target="_blank"
             class="button-link">
            View Full GANTT Schedule
          </a>
        </p>

        <iframe
          src="https://docs.google.com/spreadsheets/d/12-z6ueIstVCXI8U_jozlMdH24LCAA0FeEc4j1kiNmzk/edit?usp=sharing"
          scrolling="no">
        </iframe>
      </section>

      <!-- Footer -->
      <footer>
        &copy; 2025 CBE3300B Humidity Chamber Team
      </footer>

    </div>
  </main>

</body>
</html>
