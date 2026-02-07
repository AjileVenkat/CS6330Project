<h1>CS6330 Project: Replication of IconSched</h1>
    <p class="reference">
        <strong>Reference Paper:</strong> Improving DBMS Scheduling Decisions with Fine-grained Performance Prediction on Concurrent Queries – Extended (VLDB 2025)<br>
        <a href="https://arxiv.org/pdf/2501.16256">https://arxiv.org/pdf/2501.16256</a>
    </p>

  <h2>Team Members</h2>
    <table>
        <thead>
            <tr>
                <th>Name</th>
            </tr>
        </thead>
        <tbody>
            <tr><td>Ajay Venkat</td></tr>
            <tr><td>Ananya Palley</td></tr>
            <tr><td>Amulya Prasad Rayabhagi</td></tr>
            <tr><td>Varunika Rao Ramadugu</td></tr>
            <tr><td>Shahbaz Ali</td></tr>
        </tbody>
    </table>

  <h2>Objective</h2>
    <p>The primary objective of this project is to <strong>replicate and recreate</strong> the experimental framework of <strong>IconSched</strong>. Unlike traditional schedulers, IconSched utilizes fine-grained performance prediction to mitigate inter-query resource contention.</p>
    
  <div class="highlight">
        <strong>Key Reproduction Tasks:</strong>
        <ul>
            <li><strong>Feature Extraction:</strong> Recreating the logic to pull operator-level features from PostgreSQL query plans.</li>
            <li><strong>Model Replication:</strong> Implementing the prediction engine to estimate the interference ratio ($Latency_{concurrent} / Latency_{isolated}$).</li>
            <li><strong>Scheduler Implementation:</strong> Building the contention-aware scheduling algorithm to optimize query execution order.</li>
            <li><strong>Validation:</strong> Comparing our reproduction throughput and P99 latency results against the original VLDB 2025 paper benchmarks.</li>
        </ul>
    </div>

    

  <h2>System Architecture</h2>
    <p>Our replication follows the three-tier architecture described in the paper:</p>
    <ol>
        <li><strong>Offline Profiling:</strong> Running query pairs to train the contention-aware model.</li>
        <li><strong>Fine-grained Predictor:</strong> A machine learning model (MLP/GNN) that evaluates query plans.</li>
        <li><strong>Online Scheduler:</strong> A middleware that intercepts queries and reorders the queue based on predicted interference.</li>
    </ol>

  <h2>How to Run</h2>

  <h3>1. Environment Setup</h3>
    <p>Install the required Python packages and ensure PostgreSQL 15+ is installed:</p>
    <pre>pip install torch scikit-learn psycopg2-binary pandas</pre>

  <h3>2. Database Configuration</h3>
    <p>Load the TPC-H or TPC-DS datasets as used in the paper's evaluation section:</p>
    <pre>python scripts/setup_db.py --dataset tpch --sf 1</pre>

  <h3>3. Replication of Performance Traces</h3>
    <p>To train the predictor, run the profiling script to capture hardware-specific contention data:</p>
    <pre>python scripts/collect_data.py --samples 1000</pre>

  <h3>4. Execute IconSched</h3>
    <p>Run the main scheduling middleware to process a stream of concurrent queries:</p>
    <pre>python main.py --scheduler iconsched --workload tpch_mix</pre>

  <h3>5. Result Analysis</h3>
    <p>Generate comparison plots to verify if the replication matches the paper’s performance curves:</p>
    <pre>python scripts/generate_report.py --compare_fifo</pre>
