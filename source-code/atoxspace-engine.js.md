/\*\*  
 \* ATOXSpaceForge Pro v1.0.0 — Spacecraft Atomic Oxygen (ATOX) Erosion Engine  
 \* Standards: ECSS-E-ST-10-04C Clause 9, ASTM E2089-15, NASA-STD-6016, ISO 15856  
 \* Pure ES6 Zero-Dependency Module  
 \*/

export const ATOX\_CONSTANTS \= {  
  EARTH\_RADIUS\_KM: 6378.137,  
  STANDARD\_GRAV\_PARAM\_M3\_S2: 3.986004418e14,  
  AVOGADRO\_CONSTANT: 6.02214076e23,  
  OXYGEN\_ATOMIC\_MASS\_G\_MOL: 15.9994,  
  KAPTON\_STANDARD\_EROSION\_YIELD\_CM3\_PER\_ATOM: 3.00e-24, // ASTM E2089 reference standard  
  MIN\_REQUIRED\_THICKNESS\_MARGIN\_PCT: 33.3, // ECSS 1.5x design factor of safety requires \>= 33.3% remaining thickness  
  SILVER\_CORROSION\_CRITICAL\_FLUENCE\_ATOMS\_CM2: 1.0e20, // Silver flaking threshold  
  PINHOLE\_DEFECT\_ALLOWABLE\_DENSITY\_CM2: 10.0 // NASA-STD-6016 max pinholes  
};

export const ATOX\_PRESETS \= {  
  iss\_orbit\_cargo\_module: {  
    id: "iss\_orbit\_cargo\_module",  
    name: "400 km ISS Orbital Cargo & Science Module (Solar Max, Ram Facing)",  
    altitudeKm: 400.0,  
    inclinationDeg: 51.6,  
    missionDurationYears: 3.0,  
    solarFluxF107: 150.0,  
    ramAngleOfAttackDeg: 0.0,  
    orbitalDutyCycle: 1.0,  
    hasProtectiveCoating: true,  
    materials: \[  
      { id: "MAT-01", name: "MLI Thermal Blanket Outer Layer (Kapton HN Polyimide)", type: "thermal\_blanket", initialThicknessUm: 50.8, densityGPerCm3: 1.42, erosionYieldCm3Atom: 3.00e-24, isCoated: true, coatingType: "SiO2 Quartz Sputtered (100 nm)", coatingPinholeDensityCm2: 2.5, allowableErosionUm: 15.0 },  
      { id: "MAT-02", name: "Solar Array Structural Yoke (CFRP Epoxy Matrix)", type: "structural\_composite", initialThicknessUm: 1500.0, densityGPerCm3: 1.60, erosionYieldCm3Atom: 2.10e-24, isCoated: true, coatingType: "Aluminized Barrier Tape (25 um)", coatingPinholeDensityCm2: 1.0, allowableErosionUm: 50.0 },  
      { id: "MAT-03", name: "Solar Cell Interconnect Ribbons (Pure Silver foil)", type: "electrical\_interconnect", initialThicknessUm: 25.0, densityGPerCm3: 10.49, erosionYieldCm3Atom: 10.50e-24, isCoated: true, coatingType: "Silicone Encapsulated (DC 93-500)", coatingPinholeDensityCm2: 1.2, allowableErosionUm: 5.0 },  
      { id: "MAT-04", name: "Flexible Radiator Second-Surface Sheet (Aluminized FEP Teflon)", type: "thermal\_radiator", initialThicknessUm: 127.0, densityGPerCm3: 2.15, erosionYieldCm3Atom: 0.37e-24, isCoated: false, coatingType: "None (Direct Teflon)", coatingPinholeDensityCm2: 0.0, allowableErosionUm: 25.0 },  
      { id: "MAT-05", name: "Cable Harness Overbraid (Nomex Aramid Fiber)", type: "cable\_jacket", initialThicknessUm: 250.0, densityGPerCm3: 1.38, erosionYieldCm3Atom: 2.80e-24, isCoated: true, coatingType: "Fluoropolymer Dispersion Overcoat", coatingPinholeDensityCm2: 1.5, allowableErosionUm: 40.0 }  
    \]  
  },  
  vleo\_optical\_earth\_observer: {  
    id: "vleo\_optical\_earth\_observer",  
    name: "280 km Very Low Earth Orbit (VLEO) High-Res Optical Imager (Extreme ATOX Flux)",  
    altitudeKm: 280.0,  
    inclinationDeg: 96.5,  
    missionDurationYears: 1.0,  
    solarFluxF107: 130.0,  
    ramAngleOfAttackDeg: 15.0,  
    orbitalDutyCycle: 1.0,  
    hasProtectiveCoating: true,  
    materials: \[  
      { id: "MAT-01", name: "Aerodynamic Nosecone Skin (Corin XLS POSS-Kapton)", type: "aerodynamic\_fairing", initialThicknessUm: 75.0, densityGPerCm3: 1.45, erosionYieldCm3Atom: 0.015e-24, isCoated: true, coatingType: "POSS Silicate Nanocomposite (Inherent)", coatingPinholeDensityCm2: 0.5, allowableErosionUm: 10.0 },  
      { id: "MAT-02", name: "Forward Optics Baffle Lining (Black Kapton Polyimide)", type: "optical\_baffle", initialThicknessUm: 50.8, densityGPerCm3: 1.44, erosionYieldCm3Atom: 3.00e-24, isCoated: true, coatingType: "Al2O3 Atomic Layer Deposition (50 nm)", coatingPinholeDensityCm2: 1.8, allowableErosionUm: 12.0 },  
      { id: "MAT-03", name: "Solar Array Substrate Facesheet (Cyanate Ester CFRP)", type: "structural\_composite", initialThicknessUm: 800.0, densityGPerCm3: 1.62, erosionYieldCm3Atom: 1.80e-24, isCoated: true, coatingType: "Inorganic Oxide Barrier Foil", coatingPinholeDensityCm2: 1.0, allowableErosionUm: 30.0 },  
      { id: "MAT-04", name: "Propulsion Tank Multi-Layer Insulation (Germanium Black Kapton)", type: "thermal\_blanket", initialThicknessUm: 25.4, densityGPerCm3: 1.43, erosionYieldCm3Atom: 3.00e-24, isCoated: true, coatingType: "Germanium Vapor Deposited (60 nm)", coatingPinholeDensityCm2: 2.0, allowableErosionUm: 8.0 },  
      { id: "MAT-05", name: "External Patch Antenna Cover (Polyimide Radome)", type: "rf\_radome", initialThicknessUm: 100.0, densityGPerCm3: 1.42, erosionYieldCm3Atom: 3.00e-24, isCoated: true, coatingType: "ITO Transparent Conductive (80 nm)", coatingPinholeDensityCm2: 2.2, allowableErosionUm: 20.0 }  
    \]  
  },  
  starlink\_sso\_constellation\_bus: {  
    id: "starlink\_sso\_constellation\_bus",  
    name: "550 km Sun-Synchronous Constellation Bus (5-Year Mission, Rotating Solar Array)",  
    altitudeKm: 550.0,  
    inclinationDeg: 97.6,  
    missionDurationYears: 5.0,  
    solarFluxF107: 120.0,  
    ramAngleOfAttackDeg: 45.0,  
    orbitalDutyCycle: 1.0,  
    hasProtectiveCoating: true,  
    materials: \[  
      { id: "MAT-01", name: "Solar Array Rear Protective Film (White Tedlar / PVF)", type: "solar\_backing", initialThicknessUm: 38.1, densityGPerCm3: 1.45, erosionYieldCm3Atom: 3.20e-24, isCoated: true, coatingType: "SiO2 Barrier Coating (120 nm)", coatingPinholeDensityCm2: 2.0, allowableErosionUm: 10.0 },  
      { id: "MAT-02", name: "Bus Side Panel Thermal Blankets (1 mil Kapton HN)", type: "thermal\_blanket", initialThicknessUm: 50.8, densityGPerCm3: 1.42, erosionYieldCm3Atom: 3.00e-24, isCoated: true, coatingType: "Germanium Outer Face (80 nm)", coatingPinholeDensityCm2: 1.5, allowableErosionUm: 15.0 },  
      { id: "MAT-03", name: "Star Tracker Optical Hood Baffle (Anodized Al 6061-T6)", type: "optical\_baffle", initialThicknessUm: 1500.0, densityGPerCm3: 2.70, erosionYieldCm3Atom: 0.00e-24, isCoated: true, coatingType: "Inorganic Black Anodize", coatingPinholeDensityCm2: 0.0, allowableErosionUm: 5.0 },  
      { id: "MAT-04", name: "Inter-Satellite Laser Terminal Flex Harness (Kapton FPC)", type: "cable\_jacket", initialThicknessUm: 75.0, densityGPerCm3: 1.42, erosionYieldCm3Atom: 3.00e-24, isCoated: true, coatingType: "Polyurethane Enamel Overcoat", coatingPinholeDensityCm2: 2.5, allowableErosionUm: 15.0 },  
      { id: "MAT-05", name: "Electric Thruster Xenon Feedline Bracket (PEEK Thermoplastic)", type: "structural\_composite", initialThicknessUm: 2000.0, densityGPerCm3: 1.32, erosionYieldCm3Atom: 1.50e-24, isCoated: true, coatingType: "Aluminized Kapton Wrap", coatingPinholeDensityCm2: 1.0, allowableErosionUm: 50.0 }  
    \]  
  }  
};

export function calcAtomicOxygenDensity(altKm, f107) {  
  const h \= Math.max(180.0, Math.min(1000.0, altKm));  
  const f \= Math.max(65.0, Math.min(260.0, f107));  
    
  // Calibrated NRLMSISE-00 / ECSS-E-ST-10-04C empirical formulation  
  const hScaleKm \= 42.0 \+ 0.15 \* (f \- 70.0);  
  const n0 \= 1.8e9 \* Math.pow(f / 140.0, 0.9);  
  const nO \= n0 \* Math.exp(-(h \- 200.0) / hScaleKm);  
  const tExo \= 750.0 \+ 3.2 \* (f \- 70.0);  
    
  return {  
    nO\_atomsPerCm3: nO,  
    scaleHeightKm: Number(hScaleKm.toFixed(1)),  
    exosphericTempK: Number(tExo.toFixed(0))  
  };  
}

export function calcOrbitalVelocity(altKm) {  
  const rM \= (ATOX\_CONSTANTS.EARTH\_RADIUS\_KM \+ altKm) \* 1000.0;  
  const vMPerS \= Math.sqrt(ATOX\_CONSTANTS.STANDARD\_GRAV\_PARAM\_M3\_S2 / rM);  
  return {  
    vMPerS,  
    vKmPerS: vMPerS / 1000.0  
  };  
}

export function calcAtoxFluence(config) {  
  const atoxEnv \= calcAtomicOxygenDensity(config.altitudeKm, config.solarFluxF107);  
  const orbVel \= calcOrbitalVelocity(config.altitudeKm);  
  const vRelCmPerS \= orbVel.vMPerS \* 100.0;  
    
  const thetaRad \= (config.ramAngleOfAttackDeg \* Math.PI) / 180.0;  
  const cosTheta \= Math.max(0.0, Math.cos(thetaRad));  
  const ramFluxAtomsPerCm2S \= atoxEnv.nO\_atomsPerCm3 \* vRelCmPerS \* cosTheta;  
    
  const missionSeconds \= config.missionDurationYears \* 365.25 \* 86400.0;  
  const dutyCycle \= Number(config.orbitalDutyCycle \|\| 1.0);  
  const cumulativeFluenceAtomsPerCm2 \= ramFluxAtomsPerCm2S \* missionSeconds \* dutyCycle;  
    
  return {  
    atoxEnv,  
    orbVel,  
    cosTheta: Number(cosTheta.toFixed(3)),  
    ramFluxAtomsPerCm2S,  
    missionSeconds,  
    cumulativeFluenceAtomsPerCm2  
  };  
}

export function evaluateMaterialErosion(mat, fluenceData) {  
  const fAo \= fluenceData.cumulativeFluenceAtomsPerCm2;  
  const ey \= Number(mat.erosionYieldCm3Atom);  
    
  let effectiveEy \= ey;  
  if (mat.isCoated) {  
    const pinholeFraction \= Math.min(0.02, (mat.coatingPinholeDensityCm2 \* 1e-4));  
    effectiveEy \= ey \* pinholeFraction \+ 0.015e-24;  
  }  
    
  const recessionDepthCm \= fAo \* effectiveEy;  
  const recessionDepthUm \= recessionDepthCm \* 10000.0;  
    
  const initialThicknessUm \= Number(mat.initialThicknessUm);  
  const remainingThicknessUm \= Math.max(0.0, initialThicknessUm \- recessionDepthUm);  
  const thicknessLossPct \= (recessionDepthUm / initialThicknessUm) \* 100.0;  
  const thicknessMarginPct \= (remainingThicknessUm / initialThicknessUm) \* 100.0;  
    
  const density \= Number(mat.densityGPerCm3);  
  const massLossGPerCm2 \= recessionDepthCm \* density;  
  const massLossMgPerCm2 \= massLossGPerCm2 \* 1000.0;  
    
  const allowableUm \= Number(mat.allowableErosionUm \|\| initialThicknessUm \* 0.5);  
  const isRecessionSafe \= recessionDepthUm \<= allowableUm;  
  const isMarginCompliant \= thicknessMarginPct \>= ATOX\_CONSTANTS.MIN\_REQUIRED\_THICKNESS\_MARGIN\_PCT;  
  const isSilverSafe \= mat.type \!== "electrical\_interconnect" \|\| mat.isCoated \|\| fAo \<= ATOX\_CONSTANTS.SILVER\_CORROSION\_CRITICAL\_FLUENCE\_ATOMS\_CM2;  
    
  const isOverallCompliant \= isRecessionSafe && isMarginCompliant && isSilverSafe;  
    
  return {  
    id: mat.id,  
    name: mat.name,  
    type: mat.type,  
    initialThicknessUm,  
    erosionYieldCm3Atom: ey,  
    effectiveEyCm3Atom: effectiveEy,  
    isCoated: mat.isCoated,  
    coatingType: mat.coatingType,  
    recessionDepthUm: Number(recessionDepthUm.toFixed(2)),  
    remainingThicknessUm: Number(remainingThicknessUm.toFixed(2)),  
    thicknessLossPct: Number(thicknessLossPct.toFixed(2)),  
    thicknessMarginPct: Number(thicknessMarginPct.toFixed(2)),  
    massLossMgPerCm2: Number(massLossMgPerCm2.toFixed(3)),  
    allowableErosionUm: allowableUm,  
    isRecessionSafe,  
    isMarginCompliant,  
    isSilverSafe,  
    isOverallCompliant  
  };  
}

export function analyzeSpacecraftAtox(preset) {  
  const fluenceData \= calcAtoxFluence(preset);  
  const matResults \= preset.materials.map((m) \=\> evaluateMaterialErosion(m, fluenceData));  
    
  const maxRecessionUm \= Math.max(...matResults.map((m) \=\> m.recessionDepthUm));  
  const minThicknessMarginPct \= Math.min(...matResults.map((m) \=\> m.thicknessMarginPct));  
  const maxMassLossMgPerCm2 \= Math.max(...matResults.map((m) \=\> m.massLossMgPerCm2));  
    
  const compliantCount \= matResults.filter((m) \=\> m.isOverallCompliant).length;  
  const isSystemCompliant \= compliantCount \=== matResults.length;  
    
  return {  
    preset,  
    fluenceData,  
    maxRecessionUm: Number(maxRecessionUm.toFixed(2)),  
    minThicknessMarginPct: Number(minThicknessMarginPct.toFixed(2)),  
    maxMassLossMgPerCm2: Number(maxMassLossMgPerCm2.toFixed(3)),  
    compliantCount,  
    totalMaterials: matResults.length,  
    isSystemCompliant,  
    materials: matResults  
  };  
}  
