kicad-patches
=============

Patches for various improvements to KiCad

Latest BZR head used to create the patches: 4598

Notes:

1. To apply a patch set:
    cd your_kicad_repository
    bzr patch /path/to/these/patches/patch_to_apply.patch

2. If you apply multiple patches, the patches may conflict
    since they may make modifications to common files such as
    pcbnew/CMakeLists.txt. In such cases you may manually edit
    the file to achieve the desired result.

PATCHES:

export_vrml.patch:
    Improvements to the VRML export function.
    + the board is exported
    + somewhat realistic coloring is used
    + board is approximately centered on the world origin
    + filled zones are rendered

export_idf.patch:
    Adds basic IDFv3 export. At this stage only the board outline
    with cutouts and drill holes is exported. If you need more
    substantial IDFv3 support, please send me an email or
    ask a question via the kicad user's group.

