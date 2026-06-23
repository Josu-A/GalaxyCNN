# Galaxien sailkapena sare konboluzionalak erabiliz

Proiektu honen helburua galaxien irudien ezaugarri morfologikoak identifikatzeko gai
den CNN eredu bat sortzea da.

Honetarako erabilitako datu-multzoa hemendik lortu zen: [https://www.kaggle.com/competitions/galaxy-zoo-the-galaxy-challenge/data](https://www.kaggle.com/competitions/galaxy-zoo-the-galaxy-challenge/data).

## Proiektuaren egitura

Proiektuaren egitura honela labur daiteke:

```
.
├── data/
│   ├── training_solutions_rev1.csv                           # Entrenamendurako etiketak
│   └── images_training_rev1/                                 # Galaxien irudiak
├── docs
│   └── txostena.pdf                                          # Txosten laburra
├── Galaxien_sailkapena_konboluzio-sareak_erabiliz.ipynb      # Koaderno nagusia
├── galaxy_cnn_checkpoint/                                    # Ereduaren kontrol-puntuak
└── out/                                                      # Aldi baterako irteera-fitxategiak
```

## Konfigurazio-jarraibideak

### Aurrebaldintzak

- Python 3.12 edo bertsio bateragarria
- GPU (aukerakoa baina gomendatua):
  - CUDA euskarria duen NVIDIA GPUa (errendimendurik onena)
  - ROCm euskarria duen AMD GPUa (Linux)
  - DirectML euskarria duen edozein GPU (Windows)
  - CPU ordezkoa GPUrik ez badago erabilgarri

### Instalazioa

1. **Klonatu edo deskargatu proiektua**
   ```bash
   git clone https://github.com/Josu-A/GalaxyCNN.git
   cd GalaxyCNN
   ```
2. **Sortu ingurune birtual bat**
   ```bash
   python -m venv galaxyenv
   ```
3. **Aktibatu ingurune birtuala**
   - Windows (PowerShell):
     ```powershell
     .\galaxyenv\Scripts\Activate.ps1
     ```
   - Windows (CMD):
     ```cmd
     .\galaxyenv\Scripts\activate.bat
     ```
   - Linux/macOS:
     ```bash
     source galaxyenv/bin/activate
     ```
4. **Instalatu mendekotasunak**
   - **CUDA (NVIDIA GPU), ROCm (AMD GPU) edo CPU**:
     ```bash
     pip install -r requirements.txt
     ```
     Linuxen ROCm erabiltzeko, gehitu indizearen URLa:
     ```bash
     pip install -r requirements.txt --index-url https://download.pytorch.org/whl/rocm6.1
     ```
   - **DirectML (Windows GPU)**:
     ```bash
     pip install -r requirements-directml.txt
     ```
5. **Deskargatu datu-multzoa**
   Koadernoak datu-multzoa automatikoki deskargatuko du Google Drive-tik lehen exekuzioan, edo eskuz deskarga dezakezu [Kaggle Galaxy Zoo Challenge](https://www.kaggle.com/competitions/galaxy-zoo-the-galaxy-challenge/data) gunetik eta fitxategiak `data/` direktorioan jarri.
6. **Abiarazi Jupyter**
   ```bash
   jupyter notebook Galaxien_sailkapena_konboluzio-sareak_erabiliz.ipynb
   ```

### Eredua entrenatzea

Koadernoak pipeline osoa du: datuen kargatzea, aurreprozesatzea, ereduaren arkitekturaren definizioa, entrenamendua, ebaluazioa eta probak.