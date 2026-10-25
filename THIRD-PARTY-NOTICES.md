# Third-Party Notices — FFB Lab - R3E

The FFB Lab - R3E release package is **self-contained**: the .NET runtime and all required
libraries are bundled inside `FFB Lab - R3E.exe`. Nothing has to be installed separately.
Those components remain under their own licences, listed below.

This file satisfies the attribution requirement of those licences.

---

## Bundled inside the executable

| Component | Version | Used for | Licence |
|---|---|---|---|
| .NET Runtime and Windows Desktop (WPF) | 8.0 | the application itself, and the OpenXR layer in `OpenXR\` (compiled ahead of time) | MIT |
| SharpDX, SharpDX.DirectInput | 4.2.0 | driving the wheel (DirectInput force feedback) | MIT |
| OVRSharp | 1.0.1 | SteamVR overlay | MIT |
| OpenVR API (`openvr_api.dll`) | shipped with OVRSharp 1.0.1 | SteamVR overlay | BSD-3-Clause |
| Vortice.Direct3D11, Vortice.DXGI, Vortice.DirectX | 3.6.2 | SteamVR overlay (image sent to the headset) | MIT |
| Vortice.Mathematics | 1.9.2 | SteamVR overlay | MIT |
| SharpGen.Runtime, SharpGen.Runtime.COM | 2.2.0-beta | used by Vortice | MIT |

## Data

| Data | Source | Licence |
|---|---|---|
| `r3e-data.json` — RaceRoom car names by id | [SecondMonitor](https://github.com/Winzarten/SecondMonitor) | MIT |

---

## MIT License

Applies to:

- .NET Runtime and Windows Desktop — Copyright (c) .NET Foundation and Contributors
- SharpDX, SharpDX.DirectInput — Copyright (c) 2010-2016 Alexandre Mutel
- OVRSharp — Copyright (c) 2020-2021 TJ Horner
- Vortice.Direct3D11, Vortice.DXGI, Vortice.DirectX, Vortice.Mathematics — Copyright (c) Amer Koleci and Contributors
- SharpGen.Runtime, SharpGen.Runtime.COM — Copyright (c) 2010-2017 Alexandre Mutel, 2017-2023 Jeremy Koritzinsky, 2023-2024 Amer Koleci
- SecondMonitor (`r3e-data.json`) — Copyright (c) 2018 Matus Celko

```
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## BSD 3-Clause License — OpenVR

Copyright (c) 2015, Valve Corporation. All rights reserved.

```
Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.

2. Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation and/or
other materials provided with the distribution.

3. Neither the name of the copyright holder nor the names of its contributors
may be used to endorse or promote products derived from this software without
specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```

---

## Telemetry data

FFB Lab - R3E reads telemetry from the shared-memory block that RaceRoom Racing Experience
publishes for third-party applications. No game files are modified, patched or redistributed.
