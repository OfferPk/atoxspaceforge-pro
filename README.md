\# ATOXSpaceForge Pro v1.0.0  
\> Spacecraft Atomic Oxygen (ATOX) Erosion, Ram Fluence & Polymeric Material Lifetime Studio

\#\# Overview  
ATOXSpaceForge Pro v1.0.0 is an enterprise aerospace engineering studio developed per ECSS-E-ST-10-04C Rev. 1, ASTM E2089-15, NASA-STD-6016, and ISO 15856:2014. It provides rigorous thermospheric atomic oxygen density modeling (NRLMSISE-00), orbital velocity and ram flux integration, protective coating pinhole defect evaluation, multi-material thickness recession analysis, and automated 10-point normative compliance auditing.

\#\# Directory Structure  
\- source-code/: ES6 engine, auditor, CLI tool, test runner, and interactive HTML cockpit.  
\- build/: Standalone web studio application and distribution archive.  
\- documentation/: Standards specifications, material erosion database, executive summary, and user guides.  
\- screenshots/: Visual studio previews and atomic oxygen erosion architecture diagrams.

\#\# Quick Start  
Unit test suite:  
python3 source-code/test\_runner.py

CLI audit on ISS Cargo Module:  
python3 source-code/atoxspace\_cli.py audit \--preset iss\_orbit\_cargo\_module

Export ECSS XML Manifest:  
python3 source-code/atoxspace\_cli.py export-xml \--preset vleo\_optical\_earth\_observer  
