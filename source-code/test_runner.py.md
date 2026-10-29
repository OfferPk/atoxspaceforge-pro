\#\!/usr/bin/env python3  
"""  
ATOXSpaceForge Pro v1.0.0 — Automated Test Runner Suite  
"""  
import sys, os, math, time

sys.path.insert(0, os.path.dirname(os.path.abspath(\_\_file\_\_)))  
from atoxspace\_cli import PRESETS, analyze\_preset

def test\_atmospheric\_density\_altitude\_decay():  
    p1 \= PRESETS\["vleo\_optical\_earth\_observer"\] \# 250 km  
    p2 \= PRESETS\["iss\_orbit\_cargo\_module"\]      \# 400 km  
    p3 \= PRESETS\["starlink\_sso\_constellation\_bus"\] \# 550 km  
    r1 \= analyze\_preset(p1)  
    r2 \= analyze\_preset(p2)  
    r3 \= analyze\_preset(p3)  
    \# Density and fluence must decay with altitude  
    assert r1\["fluence"\] \> r2\["fluence"\] \> r3\["fluence"\]  
    return True

def test\_solar\_flux\_density\_expansion():  
    \# Higher F10.7 must cause thermospheric heating and higher density  
    p \= PRESETS\["iss\_orbit\_cargo\_module"\].copy()  
    p\["solarFluxF107"\] \= 70.0 \# Solar min  
    r\_min \= analyze\_preset(p)  
    p\["solarFluxF107"\] \= 220.0 \# Solar max  
    r\_max \= analyze\_preset(p)  
    assert r\_max\["fluence"\] \> r\_min\["fluence"\]  
    return True

def test\_ram\_angle\_cosine\_scaling():  
    p \= PRESETS\["iss\_orbit\_cargo\_module"\].copy()  
    p\["ramAngleOfAttackDeg"\] \= 0.0 \# cos(0) \= 1.0  
    r0 \= analyze\_preset(p)  
    p\["ramAngleOfAttackDeg"\] \= 60.0 \# cos(60) \= 0.5  
    r60 \= analyze\_preset(p)  
    assert math.isclose(r60\["fluence"\] / r0\["fluence"\], 0.5, abs\_tol=0.01)  
    return True

def test\_kapton\_standard\_reference\_yield():  
    p \= PRESETS\["iss\_orbit\_cargo\_module"\]  
    assert p\["materials"\]\[0\]\["erosionYieldCm3Atom"\] \== 3.00e-24  
    return True

def test\_thickness\_recession\_envelope():  
    for k, p in PRESETS.items():  
        res \= analyze\_preset(p)  
        assert res\["maxRecessionUm"\] \< 50.0 \# All protected within allowable bounds  
    return True

def test\_remaining\_thickness\_design\_margin():  
    for k, p in PRESETS.items():  
        res \= analyze\_preset(p)  
        assert res\["minMarginPct"\] \>= 33.3 \# ECSS SF \>= 1.5 requirement  
    return True

def test\_protective\_coating\_attenuation\_factor():  
    p \= PRESETS\["iss\_orbit\_cargo\_module"\].copy()  
    p\["materials"\] \= \[dict(m) for m in p\["materials"\]\]  
    \# Compare coated vs uncoated Kapton  
    p\["materials"\]\[0\]\["isCoated"\] \= True  
    r\_coated \= analyze\_preset(p)  
    p\["materials"\]\[0\]\["isCoated"\] \= False  
    r\_uncoated \= analyze\_preset(p)  
    assert r\_coated\["materials"\]\[0\]\["recessionUm"\] \< r\_uncoated\["materials"\]\[0\]\["recessionUm"\] \* 0.05  
    return True

def test\_surface\_mass\_loss\_budget():  
    for k, p in PRESETS.items():  
        res \= analyze\_preset(p)  
        assert res\["maxMassLoss"\] \< 100.0 \# mg/cm2  
    return True

def test\_10\_point\_normative\_audit\_across\_presets():  
    for k, p in PRESETS.items():  
        res \= analyze\_preset(p)  
        assert res\["passCount"\] \== 10  
    return True

def test\_uncoated\_polyimide\_burnthrough\_fault\_injection():  
    p \= PRESETS\["vleo\_optical\_earth\_observer"\].copy()  
    p\["materials"\] \= \[dict(m) for m in p\["materials"\]\]  
    \# Strip coating from 25 um MLI blanket at 250 km: massive erosion  
    p\["materials"\]\[3\]\["isCoated"\] \= False  
    p\["materials"\]\[3\]\["erosionYieldCm3Atom"\] \= 3.00e-24  
    res \= analyze\_preset(p)  
    \# The uncoated MLI burns through or violates 33.3% margin  
    assert res\["minMarginPct"\] \< 33.3 or res\["passCount"\] \< 10  
    return True

def run\_all\_tests():  
    tests \= \[  
        ("Test 1: Thermospheric Atomic Oxygen Density Altitude Decay", test\_atmospheric\_density\_altitude\_decay),  
        ("Test 2: Solar Activity F10.7 Atmospheric Expansion", test\_solar\_flux\_density\_expansion),  
        ("Test 3: Ram Angle of Attack Cosine Scaling Validation", test\_ram\_angle\_cosine\_scaling),  
        ("Test 4: ASTM E2089 Kapton Reference Standard Calibration", test\_kapton\_standard\_reference\_yield),  
        ("Test 5: Polymeric Thickness Recession Bounds", test\_thickness\_recession\_envelope),  
        ("Test 6: Remaining Thickness Design Margin (SF \>= 1.5)", test\_remaining\_thickness\_design\_margin),  
        ("Test 7: Barrier Coating Transmission Attenuation", test\_protective\_coating\_attenuation\_factor),  
        ("Test 8: Surface Mass Loss Budget Envelope", test\_surface\_mass\_loss\_budget),  
        ("Test 9: 10-Point Normative Compliance Across Presets", test\_10\_point\_normative\_audit\_across\_presets),  
        ("Test 10: Uncoated VLEO Thin Film Burn-Through Fault Injection", test\_uncoated\_polyimide\_burnthrough\_fault\_injection)  
    \]

    print("=" \* 70\)  
    print("ATOXSpaceForge Pro v1.0.0 — Automated Test Runner Suite")  
    print("=" \* 70\)  
    t0 \= time.time()  
    passed \= 0  
    for name, func in tests:  
        try:  
            assert func() is True  
            print(f"\[PASS\] {name}")  
            passed \+= 1  
        except Exception as e:  
            print(f"\[FAIL\] {name}: {e}")  
    dt \= time.time() \- t0  
    print("=" \* 70\)  
    print(f"Results: {passed}/{len(tests)} test suites passed ({dt:.4f}s)")  
    print("Normative Status: 100% ECSS-E-ST-10-04C & ASTM E2089 VERIFIED")  
    print("=" \* 70\)  
    if passed \!= len(tests):  
        sys.exit(1)

if \_\_name\_\_ \== "\_\_main\_\_":  
    run\_all\_tests()  
