This file is the translation of the README_j.txt file using translate.google.com.

bdf2ttf Font conversion tool manual
                                                            Since: 21-Aug-2002
                                                                  Version: 1.0
                                                  Author: MURAOKA Taro (KoRoN)
                                                     Last Change: 06-Apr-2015.


Description
  A tool for converting BDF, which is a bitmap font, to TrueType fonts.
  Since the embedded bitmap, which is a function of TrueType, is used for conversion, for conversion
  The time required is short and the converted file size is small. However, after conversion
  In general, there is a problem with display quality when used with a size other than the specified size of the BDF in which the data is embedded.
  Will occur.


how to use
  Run a command similar to the following from the command line:

    bdf2ttf [flags] {outname} {info} {infile} [infiles ...]

  [flags]
    Optional. The following options can be specified.
    -b, --bold Generate bold font.
    -i, --italic Generate italic font.
    When --no-autoname -b / i is specified, the display font name is automatically corrected.
                        Avoid.
    When --no-stylecheck -b / i is specified, the consistency check is suppressed.
  {outname}
    Required. Specify the output file name. Example: out.ttf
  {info}
    Required. Point to the font information file for setting the font name and copyright notice
    Set. The format will be described later. Example: font.ini
  {infile} [infiles ...]
    Required. Specify the input BDF file. Separate multiple items with a space
    You can do this, but you must specify at least one file. Multiple files
    And different files contain glyphs for the same character
    The latter file has priority. You can also specify different sizes of BDF
    Is possible.

  Note: A character code conversion table is required for conversion. For conversion tables
  It will be described later.

  (Font information file format)
  The font information file must be written in UTF-8 encoding. phone
  If you use only English for the name and copyright notice, you don't have to worry about it, but
  Be careful when using this term.

  The file has a structure similar to an INI file, with one key / value combination per line.
  I am. The basic format is:
      key = value or key = value
  It is the shape of. It is possible to include whitespace in keys and values, but no leading or trailing whitespace
  Will be seen. Example:
      greeting practise = Hello, World
          Key = greeting practise
           Value = Hello, World
  Lines starting with "#" are ignored as comments.

  The valid keys and their meanings are as follows.
      Copyright Copyright notice (English)
      Fontname Font name (English)
      Version Font version
      Trademark Trademark Trademark
      CopyrightCP Copyright notice (Japanese)
      FontnameCP font name (Japanese)
      TrademarkCP Trademark Display (Japanese)
      Coresize Main BDF font size
  If you do not set any keys, the default values ​​are used for each key. Core size
  When not specified, the information written in the BDF with the largest font size is used for font creation.
  To do.

  Coresize is used to collect basic information such as font aspect ratio. Unfortunately at TTF
  Cannot change the aspect ratio of font glyphs depending on their size. For it
  With bdf2ttf, you can specify multiple BDF files with different aspect ratios. Is it
  Which BDF file should the aspect ratio written in for bdf2ttf be output to TTF?
  I do not know. It is Coresize that specifies it. PIXEL_SIZE in the BDF file
  A value that can be specified as Coresize. If PIXEL_SIZE does not exist, the first parameter of SIZE
  Will be diverted.

  For example, for M + fonts, you can set 10x11 (Coresize = 10) and 12x13 (Coresize = 13).
  If not set, the maximum, in this case 13, is used. In the M + example, the difference in aspect ratio is small
  Therefore, it does not matter as much as it is reflected on the screen. However, the aspect ratio is extremely high
  The Coresize setting will come alive when embedding different fonts.

  See sample.ini for a sample font information file.

  (Character code conversion table)
  When outputting a TTF file, the character code is internally set to UNICODE. on the other hand
  In BDF, JIS is common for Japanese, but ISO-8859 series is used for English characters.
  Some of them need to be converted to UNICODE.

  Therefore, bdf2ttf uses the character code correspondence table file distributed by unicode.org.
  After processing and constructing a character code conversion table each time conversion is performed, the actual conversion is performed.
  is. The correspondence table file is usually in the current directory.
      Windows notation:. \ Ucstable.d \ *. TXT
      UNIX notation: ./ucstable.d/*.TXT
  Is searched for. If the environment variable UCSTABLEDIR is specified
      Windows notation:% UCSTABLEDIR% /*.TXT
      UNIX notation: $ UCSTABLEDIR \ *. TXT
  Search for.

  The files used are Enco, as seen under ucstable.d / in the distribution archive.
  Delete the "ISO-" at the beginning of the name, convert the alphabet to uppercase, and then add the extension
  Determine the file name according to a certain rule of adding ".TXT" or ".WIN.TXT"
  is. When using an unknown encoding, use this information in the conversion table.
  Can be added.

  (Bold / italic font)
  Normally, BDF is prepared separately for bold and italic fonts, and the -b / i flag is specified for each.
  You need to create an embedded font file for. Bold font phi
  Fonts that are not provided can be displayed because the OS automatically generates bold glyphs.
  Su. However, italic fonts are not generated and cannot be displayed.

  The ideal solution is to provide an italic font file, but as a workaround
  Normal glyphs can be forcibly used as italic glyphs. In that case
  Normally, specify the following three options when converting glyphs.
    --italic
    --no-autoname: Prevents font names from being automatically corrected when -i is specified
    --no-stylecheck: When -i is specified, it can also be used as a normal character


Source code
  The source code of bdf2ttf is available under the directory src.


Font copyright
  The rights (license conditions, copyright, etc.) of TrueType fonts converted by bdf2ttf are
  Follows the finalized BDF font. That is, BDF fonts that are prohibited from being redistributed
  Is converted to a TrueType font using bdf2ttf, the TrueType font is converted to the first
  It cannot be redistributed to three parties.

  The administrator of bdf2ttf can use bdf2ttf to convert and create TrueType fonts.
  We do not have any rights or responsibilities.


Change log
  ● 06-Apr-2015 (2.X)
    License change
    Change contact email address
    Convert Japanese text file from CP932 to UTF-8
  ● 12-Aug-2005 (2.0)
    Added option to disable style checking
    Fixed an issue where italic fonts were out of size
    Supports bold and italic fonts
    Refactor the conversion part
    Introduced bdf2_t: Changed to embed fonts of multiple sizes
    Fixed a bug that Japanese may not be used for font names
  ● 08-Feb-2003 (1.0.005)
    (1.0.006) Added compile / config_default.mk
    (1.0.005) Replace mkpkg
    (1.0.004) Test to create TTF for MacOSX
    (1.0.003) Added Makefile for Mac OS X
    (1.0.002) Corrected u22be of JISX0208.WIN.TXT to u22bf
    (1.0.001) Modified to output bloc and bdat
  ● 23-Aug-2002 (1.0)
    Release


-------------------------------------------------- -----------------------------
                  A strong will to live becomes a heart that respects a life different from oneself at the same time
                               MURAOKA Taro / Taro Muraoka <koron.kaoriya@gmail.com>
 vim: set ts = 8 sts = 2 sw = 2 tw = 78 et ft = memo: