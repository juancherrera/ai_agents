# Exercise verification

- PASS — 455 practice notebooks / 3714 code cells executed top-to-bottom with network calls and credential loading blocked.
- PASS — 254 source notebooks accounted for; 1755 source files mapped.
- PASS — 14648 local links resolved; stored notebook outputs and execution counts are empty.
- PASS — 1315 exercise files scanned for credential/key patterns; no matches.
- PASS — source fingerprints match the extracted version. Original source notebook outputs were not copied.
- PERSPECTIVE APPLIED — teaching sequence, correctness, configuration boundaries, and side effects reviewed in the authored offline exercises.
- UNVERIFIED — original hosted calls, transports, email/notifications, browser integrations, source deployments, and remote linked tutorials. They are not run by these packs.

The community maps are static source inspections. A passing simulated fixture does not verify a contribution end-to-end.
Validation ran each notebook in a fresh namespace in the project interpreter, awaiting top-level asynchronous cells; it did not launch Jupyter UI or a browser.
Python syntax notes for original source are listed in source-coverage.json. They are not silently repaired in the source.
