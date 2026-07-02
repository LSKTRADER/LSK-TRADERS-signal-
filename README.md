const express = require('express');
const { spawn } = require('child_process');
const app = express();
const PORT = process.env.PORT || 5000;

app.use(express.json());

// یہ روٹ پائتھن انجن سے سگنل لے کر آئے گا
app.get('/get-signal', (req, res) => {
    const pythonProcess = spawn('python3', ['./engine/signal_generator.py']);

    pythonProcess.stdout.on('data', (data) => {
        // پائتھن سے آنے والے آؤٹ پٹ کو JSON کے طور پر بھیج رہا ہے
        res.json({ success: true, signal: data.toString() });
    });

    pythonProcess.stderr.on('data', (data) => {
        res.status(500).json({ success: false, error: data.toString() });
    });
});

app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
