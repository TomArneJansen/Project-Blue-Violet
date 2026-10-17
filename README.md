# Project-Blue-Violet
Open-source bioelectronic interface for holistic health defense.

📑 Project Blue-Violet Frequency Response (Open-BioElec)

Welcome to the official repository for Project Blue-Violet. This is an independent, open-source bioelectronic research project dedicated to creating affordable, decentralized health-defense technology.

⚖️ JURIDISK MERKNAD / LEGAL NOTICE (Prior Art)

Oppfinner / Inventor: Tom Arne Jansen
Kontakt / Contact: tomarnejansen@outlook.com
Dato / Date: 17. oktober 2026
Dette dokumentet og den tilhørende kildekoden er herved publisert offentlig for å etablere Kjent Teknikk (Prior Art) under internasjonale patentlover.
• NORSK: Forskere, universiteter og privatpersoner har full og vederlagsfri rett til å studere, teste, bygge og videreutvikle denne teknologien. Kommersielle selskaper og profittbaserte aktører har strengt forbud mot å utnytte dette prosjektet kommersielt uten eksplisitt, skriftlig samtykke fra Tom Arne Jansen.
• ENGLISH: Universities, independent researchers, and private individuals are granted full, royalty-free rights to study, replicate, and develop this technology. Commercial entities are strictly prohibited from manufacturing, selling, or exploiting this architecture for financial gain without explicit, written consent from Tom Arne Jansen.

🔬 Om Prosjektet (Project Overview)

Project Blue-Violet benytter avansert Edge-AI i en lukket sløyfe (Closed-Loop) for å analysere kroppens biosignaler i sanntid og tilføre nøyaktige elektromagnetiske og optiske frekvenser.
Målet er å tilby en universell, rimelig maskinvare-plattform (hyllevare) som kan hjelpe og bidra innen:
• Onkologi (TTFields): Mekanisk forstyrrelse av kreftcelledeling (100 kHz – 300 kHz).
• Autoimmune sykdommer & Diabetes: Vagusnerve-stimulering (1 Hz – 30 Hz) for immunregulering, cytokin-brems og glukosehomeostase (Type 1 og Type 2 diabetes).
• Antimikrobiell forstyrrelse: Destabilisering av patogene bakterier og blokkering av virusbinding.
• Psykisk Helse & Nevro-traumer: Transkraniell mikrostrømmodulering (4 Hz – 12 Hz Theta/Alpha) for PTSD-, depresjons- og angstlindring.
• Optogenetisk synkronisering: Pulserende blålilla lys (405 nm – 450 nm) for cellulær regenerasjon.

🛠️ Maskinvare-Arkitektur (Hardware)

• Sentrale enhet: Raspberry Pi 5
• AI Akselerator: DEEPX DX-M1M NPU (eller tilsvarende ≤ 3W / 25 TOPS maskinvare)
• Frekvensgenerator: AD9833 DDS-modul (0 Hz – 12.5 MHz)
• Optisk modul: Frekvensstyrt blålilla LED-matrise

💻 Kildekode / Core AI & Frequency Loop

Denne Python-koden kjører på Raspberry Pi 5 og simulerer hvordan Edge-AI automatisk skifter mellom ulike behandlingsmoduser basert på kroppens sanntidsrespons:
python
import time
import spidev
import RPi.GPIO as GPIO

# Oppfinner: Tom Arne Jansen (tomarnejansen@outlook.com)
# Lisens: Custom Non-Commercial (PBV-NC-1.0) / CC BY-NC-SA 4.0

LED_PIN = 18
SIGNAL_TYPE_SIN = 0x0000
SIGNAL_TYPE_SQR = 0x0028

GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)

spi = spidev.SpiDev()
spi.open(0, 0)
spi.max_speed_hz = 5000000

def skriv_til_ad9833(kontroll_ord):
    msb = (kontroll_ord >> 8) & 0xFF
    lsb = kontroll_ord & 0xFF
    spi.xfer2([msb, lsb])

def sett_frekvens(frekvens_hz, bølgeform=SIGNAL_TYPE_SIN):
    freq_reg = int((frekvens_hz * 268435456) / 25000000)
    lsb_ord = (freq_reg & 0x3FFF) | 0x4000
    msb_ord = ((freq_reg >> 14) & 0x3FFF) | 0x4000
    
    skriv_til_ad9833(0x2100)
    skriv_til_ad9833(lsb_ord)
    skriv_til_ad9833(msb_ord)
    skriv_til_ad9833(bølgeform)

def synkronisert_blålilla_lys(frekvens_hz, varighet_sek):
    p_vifte = GPIO.PWM(LED_PIN, frekvens_hz)
    p_vifte.start(50)
    time.sleep(varighet_sek)
    p_vifte.stop()

def simulated_edge_ai_loop():
    print(f"[{time.strftime('%H:%M:%S')}] Project Blue-Violet AI-sløyfe startet av Tom Arne Jansen.")
    
    # MODUS 1: Onkologi
    print("\n[AI MODUS] Detekterer patologisk celledeling. Aktiverer TTFields-forstyrrelse.")
    sett_frekvens(150000, SIGNAL_TYPE_SIN)
    time.sleep(3)
    
    # MODUS 2: Autoimmunitet & Diabetes
    print("\n[AI MODUS] Høyt systemisk inflammasjonsnivå/blodsukker målt. Skifter til Vagus-stimulering.")
    sett_frekvens(10, SIGNAL_TYPE_SQR)
    synkronisert_blålilla_lys(10, 4)
    
    # MODUS 3: Psykisk Helse
    print("\n[AI MODUS] Høyt stresspotensial (Amygdala) detektert. Aktiverer Theta-bølgesynkronisering.")
    sett_frekvens(6, SIGNAL_TYPE_SIN)
    time.sleep(3)

try:
    while True:
        simulated_edge_ai_loop()
        print("\n--- AI fullførte skanning. Starter ny lukket sløyfe-analyse. ---\n")
        time.sleep(2)
except KeyboardInterrupt:
    print("\nSystemet avsluttet trygt.")
finally:
    spi.close()
    GPIO.cleanup()
Vær forsiktig når du bruker koden
For questions, collaboration requests, or commercial licensing inquiries, please contact Tom Arne Jansen at tomarnejansen@outlook.com.
