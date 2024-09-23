---
layout: page
---

# G4cgalboolean

### Introduction
The Geant4 boolean processor often fails to compute a resulting mesh. There are other meshing tools available but many suffer
from robustness issues. g4cgalboolean uses [CGAL](https://www.cgal.org) mesh
[boolean and refinement](https://doc.cgal.org/latest/Polygon_mesh_processing/index.html#Coref_section) to perform boolean operations. The
CGAL license is not compatible with Geant4 license and so is distributed separately from Geant4. If your Geant4 application is compatible with
GPL then you are free to distribute g4cgal in accordance with that license. 

 * [Source code repository](https://github.com/g4edge/g4cgalboolean)

### Implementation
An virtual base class `G4VBooleanProcessor` can be implemented to provide the
boolean processing functionality. In the case of `g4cgalboolean` this class is called `G4BooleanProcessorCGAL` and Geant4 can be informed to use it via a static method of `G4Boolean` so

```
...
#include "G4BooleanProcessorCGAL.hh"
...

int main(int argc, char** argv) {
  G4BooleanSolid::SetExternalBooleanProcessor(new G4BooleanProcessorCGAL());
  ...
  ...
```

### Using g4cgalboolean in your project
1. Copy the `src` and `include` code over to your geant4 application (`G4BooleanProcessorCGAL`, `G4SurfaceMeshCGAL` and `G4VSurfaceMesh`)
1. Install CGAL using your package manager 
1. Add CGAL to your [`CMakeLists.txt`](https://github.com/g4edge/g4cgalboolean/blob/main/CMakeLists.txt) see example in github repository
1. Compile and enjoy
