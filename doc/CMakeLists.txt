cmake_minimum_required(VERSION 2.6)

# Building of Documentation is tested on Linux only
if(CMAKE_SYSTEM_NAME STREQUAL Linux)

# adapted from http://github.com/purpleKarrot/CToolBox.git
  set(THIS_PROJECT_NAME aboria)
  set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})
  find_package(Reference)
  find_package(Documentation)

  set(ABORIA_HEADERS
    ../src/Aboria.h
    ../src/Particles.h
    ../src/Variable.h
    ../src/DataVector.h
    ../src/BucketSearchParallel.h
    ../src/Functions.h
    ../src/Log.h
    ../src/Symbolic.h
    ../src/Traits.h
    ../src/Get.h
    ../src/Assemble.h
    ../src/Operators.h
    ../src/Vector.h
    )

  add_reference(libaboria.xml ${ABORIA_HEADERS} BOOSTBOOK boost.doxygen.reftitle="libaboria")
  
    set(QBK_SOURCES
        aboria.qbk
        installation.qbk
        neighbourhood.qbk
        container.qbk
        introduction.qbk
        reference.qbk
        examples.qbk
        linear_algebra.qbk
        symbolic.qbk
        )

    set (HTML_XSL_PRE "<?xml version='1.0'?>
<xsl:stylesheet xmlns:xsl=\"http://www.w3.org/1999/XSL/Transform\" version=\"1.0\">
<xsl:import href=\"${BOOSTBOOK_XSL_DIR}/html.xsl\"/>
<xsl:template name=\"user.head.content\">
<script type=\"text/x-mathjax-config\">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\\\(','\\\\)']]}
});
</script>
<script type=\"text/javascript\" 
  src=\"https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_CHTML\">
</script>
")
    set (HTML_XSL_GOOGLE_ANALYTICS "
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-48149829-4', 'auto');
  ga('send', 'pageview');
</script>")

    set (HTML_XSL_POST "</xsl:template>
</xsl:stylesheet>")

   file(WRITE html.xsl "${HTML_XSL_PRE}${HTML_XSL_POST}")
   file(WRITE html-website.xsl "${HTML_XSL_PRE}${HTML_XSL_GOOGLE_ANALYTICS}${HTML_XSL_POST}")

    # PARAMETERS : http://docbook.sourceforge.net/release/xsl/current/doc/
  add_documentation(
  #  quickstart.cpp
    ${QBK_SOURCES}
    libaboria.xml
  PARAMETERS
    #boost.root=./
    html.stylesheet=css/boostbook.css
    boost.image.src=images/aboria2.jpg
    boost.image.alt="Aboria"
    boost.image.w=200
    boost.image.h=62
    boost.mathjax=1
    chunk.section.depth=1
    #<xsl:param>chunk.first.sections=1
    #toc.section.depth=3
    #toc.max.depth=2
    #generate.section.toc.level=4
    nav.layout=none
    #admon.graphics.path=images/admonition/
    #navig.graphics.path=images/navigation/
    #callout.graphics.path=images/callout/
    #boost.doxygen.strip_from_path=.
    #img.src.path=../images/useless_directory/
  IMAGES
    images/aboria2.png
    images/next.png
    images/prev.png
    images/up.png
    images/down.png
    images/home.png
    css/boostbook.css
  )

endif(CMAKE_SYSTEM_NAME STREQUAL Linux)
