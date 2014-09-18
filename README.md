kicad-patches
=============

Patches for various improvements to KiCad

Latest BZR head used to create the patches: 4657

Notes:

1. To apply a patch set:
    cd your_kicad_repository
    bzr patch /path/to/these/patches/patch_to_apply.patch

2. If you apply multiple patches, the patches may conflict
    since they may make modifications to common files such as
    pcbnew/CMakeLists.txt. In such cases you may manually edit
    the file to achieve the desired result.

PATCHES:

underlay_in_progress.patch: [rev. 5140]
    This patch adds two Underlay layers to aid in reconstructing
    PCBs from existing digitized artwork files such as a photograph
    of an existing board or printouts from a magazine. This is a
    work in progress and is not yet suitable for inclusion in the
    main KiCad branch. At this point in time the bitmap2component
    tool can export an image to the Front Silk layer (the only
    option in the past) as well as one of two Underlay layers.
    In pcbnew the Underlay layers are actually rendered on screen
    last (they're overlays, not underlays) but this patch has been
    released for users who might want to try the tool in its
    current condition.

DEPRECATED PATCHES:

vrml_layer_pth.patch: [rev. 4942]
    This patch adds plate-through holes to the exported VRML file
    and fixes potential segfaults arising from issues in the
    GLU tesselator. Known issue: plated holes on the board edge
    are not rendered well; see the pic_programmer demo project
    for an example of this. Plated holes on the edge of an
    interior cutout will produce even uglier results.

kicad_vrml_export.patch:
    This patch is a rewrite of the VRML export routine. The
    supporting code for VRML manipulation has been cleaned
    up and a few rendering bugs fixed. Some rendering bugs
    remain but these occur infrequently, are only cosmetic,
    and their cause is now understood so they can be fixed
    in a future patch.

kicad_idf_export.patch: [ rev. 4919 ]
    This patch fixes a number of bugs in the IDF library and
    reimplements the IDFv3 export using the new IDF library.
    The old minimal IDF3 support has been removed. The new
    IDF export was tested by exporting a number of existing
    projects and manually comparing the resulting IDF files.

kicad_idftools.patch: [DEPRECATED; code has been merged into KiCad]
    This is a work in progress and is stored here primarily
    for backup purposes.  The IDF framework has been rewritten
    to support READ/WRITE.  The improved framework can in principle
    be used for KiCad IDF import/export as well as generic IDF
    tools such as the IDF to VRML converter (work in progress).

idf_tools.patch: [DEPRECATED; code has been merged into KiCad]
    Adds tools for creating IDF component outlines:
        1. idfcyl: creates a cylindrical outline in horizontal or
            vertical orientation and with axial or radial leads.
        2. idfrect: creates a rectangular outline which may have
            axial leads or a chamfer in the top left corner.
        3. dxf2idf: converts lines, arcs, and circles in a DXF file
            into an IDF outline.
    Other changes:
        + pcbnew/exporters/idf.{cpp, h} have been refactored; objects
            in common with the dxf2idf tool have been moved into
            pcbnew/exporters/idf_common.{cpp, h}

export_vrml.patch: [DEPRECATED; code is KiCad source since rev. 4611]
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
        + changed 'errno' to 'errorID' in vrml_board.h

export_idf.patch: [DEPRECATED; code is now in KiCad source since rev. 4600]
    Adds basic IDFv3 export. At this stage only the board outline
    with cutouts and drill holes is exported. If you need more
    substantial IDFv3 support, please send me an email or
    ask a question via the kicad user's group.

export_idf3_libs.patch: [DEPRECATED; code in KiCad source since rev. 4633]
    Patch for KiCad Rev. 4632
    Adds support for IDF component files. Combined with the existing
    IDF board only export, KiCad now has basic IDF support required for
    interaction with mechanical designers.

    Other changes:
        Class S3D_MASTER now protects m_Shape3DName via accessors
        GetShape3DName() and SetShape3DName(). This was done to
        aid the implementation of Is3DType() which uses an enum
        to indicate the type of 3D file based on extension;
        ".wrl" and ".x3d" == S3D_MASTER::FILE3D_VRML,
        ".idf" == S3D_MASTER::FILE3D_IDF. This list is expected to
        grow in the future as MCAD exporters are developed.
        There is a small advantage to using an enum in the
        Is3DType() function; any routine invoking the function
        can pass the enum S3D_MASTER::FILE3D_UNKNOWN to check if
        the filename has an extension currently recognized by
        S3D_MASTER. Another advantage is that the extension
        comparison is done only once when the file name is set.
        If Is3DType() used explicit extension comparisons then we
        would either have to pass a list of extensions to the
        function or we would need to call the function multiple
        times in the case of VRML related files (*.wrl,*.x3d).
        Also, in the future an MCAD exporter may wish to access
        the enum via an accessor so that the value can be used
        in a switch() statement.
