# LSK-TRADERS-signal-
Advanced AI-driven trading signals engine for Forex and Crypto markets. Built with TypeScript and Python to provide automated technical analysis and institutional-grade trading insights،An AI-powered multi-asset trading platform designed for real-time signal generation using SMC, ICT, and Supply &amp; Demand strategies

// backend/server.js
const express = require('express');
const { spawn } = require('child_process'); // پائتھن انجن کو چلانے کے لیے
const app = express();
const PORT = process.env.PORT || 5000;

app.use(express.json());

// یہ روٹ پائتھن انجن سے سگنل لے کر آئے گا
app.get('/get-signal', (req, res) => {
    const pythonProcess = spawn('python3', ['./engine/signal_generator.py']);

    pythonProcess.stdout.on('data', (data) => {
        // سگنل کو JSON میں تبدیل کر کے واپس بھیج رہا ہے
        res.json({ success: true, signal: data.toString() });
    });

    pythonProcess.stderr.on('data', (data) => {
        res.status(500).json({ success: false, error: data.toString() });
    });
});

app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
