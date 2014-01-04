kicad-patches
=============

Patches for various improvements to KiCad

Latest BZR head used to create the patches: 4601

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

    Revisions:
        bzr rev 4601:
        + included changes suggested by JP Charras for compilation
          in MinGW
        + fixed bug in export of board cutouts (cutouts not rendered)
        + fixed bug in PCB text export (premature return resulting
          in text not rendered)
        + changed some functions to 'static' to reduce name pollution

export_idf.patch: [DEPRECATED; code is now in KiCad source since rev. 4600]
    Adds basic IDFv3 export. At this stage only the board outline
    with cutouts and drill holes is exported. If you need more
    substantial IDFv3 support, please send me an email or
    ask a question via the kicad user's group.
