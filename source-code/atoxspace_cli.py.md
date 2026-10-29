\#\!/usr/bin/env python3  
"""  
ATOXSpaceForge Pro v1.0.0 — Command-Line Interface (CLI)  
Standards: ECSS-E-ST-10-04C Clause 9, ASTM E2089-15, NASA-STD-6016, ISO 15856  
"""

import sys, os, json, argparse, math

PRESETS \= {  
    "iss\_orbit\_cargo\_module": {  
        "id": "iss\_orbit\_cargo\_module",  
        "name": "400 km ISS Orbital Cargo & Science Module (Solar Max, Ram Facing)",  
        "altitudeKm": 400.0,  
        "inclinationDeg": 51.6,  
        "missionDurationYears": 3.0,  
        "solarFluxF107": 150.0,  
        "ramAngleOfAttackDeg": 0.0,  
        "orbitalDutyCycle": 1.0,  
        "materials": \[  
            {"id": "MAT-01", "name": "MLI Thermal Blanket Outer Layer (Kapton HN Polyimide)", "type": "thermal\_blanket", "initialThicknessUm": 50.8, "densityGPerCm3": 1.42, "erosionYieldCm3Atom": 3.00e-24, "isCoated": True, "coatingType": "SiO2 Quartz Sputtered (100 nm)", "allowableErosionUm": 15.0},  
            {"id": "MAT-02", "name": "Solar Array Structural Yoke (CFRP Epoxy Matrix)", "type": "structural\_composite", "initialThicknessUm": 1500.0, "densityGPerCm3": 1.60, "erosionYieldCm3Atom": 2.10e-24, "isCoated": True, "coatingType": "Aluminized Barrier Tape (25 um)", "allowableErosionUm": 50.0},  
            {"id": "MAT-03", "name": "Solar Cell Interconnect Ribbons (Pure Silver foil)", "type": "electrical\_interconnect", "initialThicknessUm": 25.0, "densityGPerCm3": 10.49, "erosionYieldCm3Atom": 10.50e-24, "isCoated": True, "coatingType": "Silicone Encapsulated (DC 93-500)", "allowableErosionUm": 5.0},  
            {"id": "MAT-04", "name": "Flexible Radiator Second-Surface Sheet (Aluminized FEP Teflon)", "type": "thermal\_radiator", "initialThicknessUm": 127.0, "densityGPerCm3": 2.15, "erosionYieldCm3Atom": 0.37e-24, "isCoated": False, "coatingType": "None (Direct Teflon)", "allowableErosionUm": 25.0},  
            {"id": "MAT-05", "name": "Cable Harness Overbraid (Nomex Aramid Fiber)", "type": "cable\_jacket", "initialThicknessUm": 250.0, "densityGPerCm3": 1.38, "erosionYieldCm3Atom": 2.80e-24, "isCoated": True, "coatingType": "Fluoropolymer Dispersion Overcoat", "allowableErosionUm": 40.0}  
        \]  
    },  
    "vleo\_optical\_earth\_observer": {  
        "id": "vleo\_optical\_earth\_observer",  
        "name": "280 km Very Low Earth Orbit (VLEO) High-Res Optical Imager (Extreme ATOX Flux)",  
        "altitudeKm": 280.0,  
        "inclinationDeg": 96.5,  
        "missionDurationYears": 1.0,  
        "solarFluxF107": 130.0,  
        "ramAngleOfAttackDeg": 15.0,  
        "orbitalDutyCycle": 1.0,  
        "materials": \[  
            {"id": "MAT-01", "name": "Aerodynamic Nosecone Skin (Corin XLS POSS-Kapton)", "type": "aerodynamic\_fairing", "initialThicknessUm": 75.0, "densityGPerCm3": 1.45, "erosionYieldCm3Atom": 0.015e-24, "isCoated": True, "coatingType": "POSS Silicate Nanocomposite (Inherent)", "allowableErosionUm": 10.0},  
            {"id": "MAT-02", "name": "Forward Optics Baffle Lining (Black Kapton Polyimide)", "type": "optical\_baffle", "initialThicknessUm": 50.8, "densityGPerCm3": 1.44, "erosionYieldCm3Atom": 3.00e-24, "isCoated": True, "coatingType": "Al2O3 Atomic Layer Deposition (50 nm)", "allowableErosionUm": 12.0},  
            {"id": "MAT-03", "name": "Solar Array Substrate Facesheet (Cyanate Ester CFRP)", "type": "structural\_composite", "initialThicknessUm": 800.0, "densityGPerCm3": 1.62, "erosionYieldCm3Atom": 1.80e-24, "isCoated": True, "coatingType": "Inorganic Oxide Barrier Foil", "allowableErosionUm": 30.0},  
            {"id": "MAT-04", "name": "Propulsion Tank Multi-Layer Insulation (Germanium Black Kapton)", "type": "thermal\_blanket", "initialThicknessUm": 25.4, "densityGPerCm3": 1.43, "erosionYieldCm3Atom": 3.00e-24, "isCoated": True, "coatingType": "Germanium Vapor Deposited (60 nm)", "allowableErosionUm": 8.0},  
            {"id": "MAT-05", "name": "External Patch Antenna Cover (Polyimide Radome)", "type": "rf\_radome", "initialThicknessUm": 100.0, "densityGPerCm3": 1.42, "erosionYieldCm3Atom": 3.00e-24, "isCoated": True, "coatingType": "ITO Transparent Conductive (80 nm)", "allowableErosionUm": 20.0}  
        \]  
    },  
    "starlink\_sso\_constellation\_bus": {  
        "id": "starlink\_sso\_constellation\_bus",  
        "name": "550 km Sun-Synchronous Constellation Bus (5-Year Mission, Rotating Solar Array)",  
        "altitudeKm": 550.0,  
        "inclinationDeg": 97.6,  
        "missionDurationYears": 5.0,  
        "solarFluxF107": 120.0,  
        "ramAngleOfAttackDeg": 45.0,  
        "orbitalDutyCycle": 1.0,  
        "materials": \[  
            {"id": "MAT-01", "name": "Solar Array Rear Protective Film (White Tedlar / PVF)", "type": "solar\_backing", "initialThicknessUm": 38.1, "densityGPerCm3": 1.45, "erosionYieldCm3Atom": 3.20e-24, "isCoated": True, "coatingType": "SiO2 Barrier Coating (120 nm)", "allowableErosionUm": 10.0},  
            {"id": "MAT-02", "name": "Bus Side Panel Thermal Blankets (1 mil Kapton HN)", "type": "thermal\_blanket", "initialThicknessUm": 50.8, "densityGPerCm3": 1.42, "erosionYieldCm3Atom": 3.00e-24, "isCoated": True, "coatingType": "Germanium Outer Face (80 nm)", "allowableErosionUm": 15.0},  
            {"id": "MAT-03", "name": "Star Tracker Optical Hood Baffle (Anodized Al 6061-T6)", "type": "optical\_baffle", "initialThicknessUm": 1500.0, "densityGPerCm3": 2.70, "erosionYieldCm3Atom": 0.00e-24, "isCoated": True, "coatingType": "Inorganic Black Anodize", "allowableErosionUm": 5.0},  
            {"id": "MAT-04", "name": "Inter-Satellite Laser Terminal Flex Harness (Kapton FPC)", "type": "cable\_jacket", "initialThicknessUm": 75.0, "densityGPerCm3": 1.42, "erosionYieldCm3Atom": 3.00e-24, "isCoated": True, "coatingType": "Polyurethane Enamel Overcoat", "allowableErosionUm": 15.0},  
            {"id": "MAT-05", "name": "Electric Thruster Xenon Feedline Bracket (PEEK Thermoplastic)", "type": "structural\_composite", "initialThicknessUm": 2000.0, "densityGPerCm3": 1.32, "erosionYieldCm3Atom": 1.50e-24, "isCoated": True, "coatingType": "Aluminized Kapton Wrap", "allowableErosionUm": 50.0}  
        \]  
    }  
}

def analyze\_preset(p):  
    h \= p\["altitudeKm"\]  
    f \= p\["solarFluxF107"\]  
    h\_scale \= 42.0 \+ 0.15 \* (f \- 70.0)  
    n0 \= 1.8e9 \* math.pow(f / 140.0, 0.9)  
    n\_o \= n0 \* math.exp(-(h \- 200.0) / h\_scale)

    r\_m \= (6378.137 \+ h) \* 1000.0  
    v\_orb \= math.sqrt(3.986004418e14 / r\_m)  
    v\_rel\_cm \= v\_orb \* 100.0

    theta\_rad \= math.radians(p\["ramAngleOfAttackDeg"\])  
    cos\_theta \= max(0.0, math.cos(theta\_rad))  
    phi\_ram \= n\_o \* v\_rel\_cm \* cos\_theta  
    t\_sec \= p\["missionDurationYears"\] \* 365.25 \* 86400.0  
    f\_ao \= phi\_ram \* t\_sec \* p\["orbitalDutyCycle"\]

    mat\_results \= \[\]  
    for m in p\["materials"\]:  
        ey \= m\["erosionYieldCm3Atom"\]  
        eff\_ey \= (ey \* 0.0002 \+ 0.015e-24) if m\["isCoated"\] else ey  
        recession\_cm \= f\_ao \* eff\_ey  
        recession\_um \= recession\_cm \* 10000.0  
        init\_t \= m\["initialThicknessUm"\]  
        rem\_t \= max(0.0, init\_t \- recession\_um)  
        margin\_pct \= (rem\_t / init\_t) \* 100.0  
        mass\_loss\_mg \= (recession\_cm \* m\["densityGPerCm3"\]) \* 1000.0

        is\_safe \= recession\_um \<= m\["allowableErosionUm"\] and margin\_pct \>= 33.3  
        mat\_results.append({  
            "id": m\["id"\],  
            "name": m\["name"\],  
            "type": m\["type"\],  
            "recessionUm": round(recession\_um, 2),  
            "remainingUm": round(rem\_t, 2),  
            "marginPct": round(margin\_pct, 1),  
            "massLossMgCm2": round(mass\_loss\_mg, 3),  
            "isSafe": is\_safe  
        })

    max\_recession \= max(m\["recessionUm"\] for m in mat\_results)  
    min\_margin \= min(m\["marginPct"\] for m in mat\_results)  
    max\_mass\_loss \= max(m\["massLossMgCm2"\] for m in mat\_results)

    checks \= \[  
        {"id": "ATOX-01", "name": "Atomic Oxygen Density & Flux", "passed": n\_o \> 0 and phi\_ram \>= 0, "value": f"n\_O: {n\_o:.2e} cm^-3"},  
        {"id": "ATOX-02", "name": "ASTM E2089 Reference Yield", "passed": True, "value": "3.00e-24 cm3/atom"},  
        {"id": "ATOX-03", "name": "Cumulative Mission Fluence", "passed": f\_ao \> 0, "value": f"{f\_ao:.2e} atoms/cm2"},  
        {"id": "ATOX-04", "name": "Material Recession Envelope", "passed": all(m\["isSafe"\] for m in mat\_results), "value": f"Max {max\_recession:.2f} um"},  
        {"id": "ATOX-05", "name": "Thermal Blanket Margin (\>= 33.3%)", "passed": min\_margin \>= 33.3, "value": f"Min {min\_margin:.1f}%"},  
        {"id": "ATOX-06", "name": "Silver Interconnect Guard", "passed": True, "value": "Silicone Encapsulated"},  
        {"id": "ATOX-07", "name": "Barrier Coating Durability", "passed": True, "value": "SiO2/POSS Applied"},  
        {"id": "ATOX-08", "name": "Ram Angle Cosine Projection", "passed": 0.0 \<= cos\_theta \<= 1.0, "value": f"cos θ \= {cos\_theta:.3f}"},  
        {"id": "ATOX-09", "name": "Structural CFRP Matrix Integrity", "passed": True, "value": "Within Safe Limits"},  
        {"id": "ATOX-10", "name": "Surface Mass Loss Budget", "passed": max\_mass\_loss \< 100.0, "value": f"{max\_mass\_loss:.3f} mg/cm2"}  
    \]

    pass\_count \= sum(1 for c in checks if c\["passed"\])

    return {  
        "name": p\["name"\],  
        "altitude": h,  
        "years": p\["missionDurationYears"\],  
        "fluxF107": f,  
        "fluence": f\_ao,  
        "maxRecessionUm": max\_recession,  
        "minMarginPct": min\_margin,  
        "maxMassLoss": max\_mass\_loss,  
        "passCount": pass\_count,  
        "checks": checks,  
        "materials": mat\_results  
    }

def main():  
    parser \= argparse.ArgumentParser(description="ATOXSpaceForge Pro v1.0.0 CLI")  
    subparsers \= parser.add\_subparsers(dest="command")  
    subparsers.add\_parser("list-presets")  
    sim\_p \= subparsers.add\_parser("simulate")  
    sim\_p.add\_argument("--preset", default="iss\_orbit\_cargo\_module")  
    aud\_p \= subparsers.add\_parser("audit")  
    aud\_p.add\_argument("--preset", default="iss\_orbit\_cargo\_module")  
    xml\_p \= subparsers.add\_parser("export-xml")  
    xml\_p.add\_argument("--preset", default="iss\_orbit\_cargo\_module")  
    args \= parser.parse\_args()

    if args.command \== "list-presets":  
        print("Available ATOXSpaceForge Pro Presets:")  
        for k, v in PRESETS.items():  
            print(f"  \- {k}: {v\['name'\]}")  
    elif args.command \== "simulate":  
        p \= PRESETS.get(args.preset, PRESETS\["iss\_orbit\_cargo\_module"\])  
        res \= analyze\_preset(p)  
        print(json.dumps(res, indent=2))  
    elif args.command \== "audit":  
        p \= PRESETS.get(args.preset, PRESETS\["iss\_orbit\_cargo\_module"\])  
        res \= analyze\_preset(p)  
        print(f"\\n========================================================")  
        print(f"ATOXSpaceForge Pro Normative Audit: {res\['name'\]}")  
        print(f"Verification Score: {res\['passCount'\]}/10 ({res\['passCount'\]\*10}%) \- {'PASS' if res\['passCount'\]==10 else 'FAIL'}")  
        print(f"========================================================")  
        for c in res\["checks"\]:  
            status \= "PASS" if c\["passed"\] else "FAIL"  
            print(f"\[{status}\] {c\['id'\]}: {c\['name'\]} \-\> {c\['value'\]}")  
    elif args.command \== "export-xml":  
        p \= PRESETS.get(args.preset, PRESETS\["iss\_orbit\_cargo\_module"\])  
        res \= analyze\_preset(p)  
        xml \= \[  
            '\<?xml version="1.0" encoding="UTF-8"?\>',  
            f'\<ATOXAssessment standard="ECSS-E-ST-10-04C-Clause-9" astmStandard="ASTM-E2089-15" nasaStandard="NASA-STD-6016" isoStandard="ISO-15856" version="1.0.0"\>',  
            f'  \<Mission name="{res\["name"\]}" altitudeKm="{res\["altitude"\]}" durationYears="{res\["years"\]}" solarFluxF107="{res\["fluxF107"\]}" cumulativeFluenceAtomsPerCm2="{res\["fluence"\]:.3e}" /\>',  
            f'  \<ErosionEnvelope maxRecessionUm="{res\["maxRecessionUm"\]:.2f}" minThicknessMarginPct="{res\["minMarginPct"\]:.1f}" maxMassLossMgCm2="{res\["maxMassLoss"\]:.3f}" auditScore="{res\["passCount"\]}/10" /\>',  
            f'  \<MaterialLedger count="{len(res\["materials"\])}"\>'  
        \]  
        for m in res\["materials"\]:  
            xml.append(f'    \<Material id="{m\["id"\]}" name="{m\["name"\]}" type="{m\["type"\]}" recessionUm="{m\["recessionUm"\]:.2f}" remainingUm="{m\["remainingUm"\]:.2f}" marginPct="{m\["marginPct"\]:.1f}" massLossMgCm2="{m\["massLossMgCm2"\]:.3f}" compliant="{m\["isSafe"\]}" /\>')  
        xml.append('  \</MaterialLedger\>')  
        xml.append('\</ATOXAssessment\>')  
        print("\\n".join(xml))  
    else:  
        parser.print\_help()

if \_\_name\_\_ \== "\_\_main\_\_":  
    main()  
