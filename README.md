# Galaxien sailkapena sare konboluzionalak erabiliz

Proiektu honen helburua galaxien irudien ezaugarri morfologikoak identifikatzeko gai
den CNN eredu bat sortzea da.

Honetarako erabilitako datu-multzoa hemendik lortu zen: [https://www.kaggle.com/competitions/galaxy-zoo-the-galaxy-challenge/data](https://www.kaggle.com/competitions/galaxy-zoo-the-galaxy-challenge/data).

## Zer da

GalaxyCNN galaxien irudi bat hartzen duen eta haren morfologia sailkatzen duen
konboluzio-sare bat (CNN) da: leuna, diskoduna edo izarra den, ertza ikusten
den, tontorrik duen, etab. Galaxy Zoo datu-multzoa erabiliz entrenatu da, non
boluntarioek hamaika galdereko erabaki-zuhaitz bati erantzunez galaxiak sailkatu
zituzten. Erantzun hauek bildu eta 37 probabilitate-sailkapen bihurtu ziren.

## Funtzionamendua

- **ResNet-50 arkitektura zerotik**: *bottleneck* hondar-blokeak [3, 4, 6, 3]
egituran, dimentsioak aldatzen direnean 1x1 konboluzioko proiekzio-lasterbideak,
Kaiming hasieraketa. Guztira 23,6 milioi parametro.
- **Irteera bat galdera bakoitzarentzat**: Lehenengo galderen erantzunen
gehiketak 1 emateko *softmax* erabiltzen da. Erabaki-zuhaitzaren egitura
errespetatzeko gainontzeko galderen buruak bertara daraman erantzunaren
probabilitatearekin eskalatzen dira.
- **Aurretratamendua**: irudiak 256x256 tamainara erditik mozten dira eta 128x128
tamainara txikitzen dira. Entrenamendu irudi-multzoaren batezbestekoa eta
desbideratzen estandarra erabiliz pixelak normalizatzen dira.
- **Entrenamendua**: MSE loss, Adam (0,001eko ikaskuntza-tasa, 0,0005eko pisuen
gainbera), ReduceLROnPlateau eta gelditze goiztiarra, berregingarritasuna
bermatzeko hazi finkoa. Entrenamendu irudiak ausaz biratzen eta jaulkitzen dira,
baita ere argitasuna eta kontrastea ausaz aldatu.

## Emaitzak

61.578 irudietatik ausaz hautatutako 15.000 irudiko azpimultzo bat 70/15/15
proportzioan banatu zen: entrenamendua (10.500), balioztatze (2.250) eta proba
(2.250). 100 epokaren ondoren (GPU batean 2,5 ordu inguru):

| Metrika | Proba-multzoa |
|---------|---------------|
| MSE     | 0,00996       |
| MAE     | 0,0633        |

Batez beste, aurreikusitako probabilitate bakoitza boluntarioen erantzunetatik 6
puntu portzentual ingurura dago. Balioztapeneko eta probako emaitzak ia berdinak
dira; beraz, ereduak gainegokitzapenik gabe orokortzen du.

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
