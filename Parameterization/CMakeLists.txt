CMAKE_MINIMUM_REQUIRED(VERSION 2.4)

PROJECT( Parameterization )

# On Visual Studio 8 MS deprecated C. This removes all 1.276E1265 security
# warnings
IF(NOT BORLAND)
 IF(NOT CYGWIN)
   IF(NOT MINGW)
     IF(NOT ITK_ENABLE_VISUAL_STUDIO_DEPRECATED_C_WARNINGS)
       ADD_DEFINITIONS(
         -D_CRT_NONSTDC_NO_DEPRECATE
         -D_CRT_SECURE_NO_DEPRECATE
         -D_SCL_SECURE_NO_DEPRECATE
         )
     ENDIF(NOT ITK_ENABLE_VISUAL_STUDIO_DEPRECATED_C_WARNINGS)
   ENDIF(NOT MINGW)
 ENDIF(NOT CYGWIN)
ENDIF(NOT BORLAND)

FIND_PACKAGE(ITK)

IF(ITK_FOUND)
    INCLUDE(${ITK_USE_FILE})
ENDIF(ITK_FOUND)

INCLUDE_DIRECTORIES( BEFORE ${ITK_INCLUDE_DIR} )

INCLUDE( CTest )

FIND_PATH( DATAROOT mushroom.vtk
  ${Parameterization_SOURCE_DIR}/data
  $ENV{DATAROOT} )

ADD_EXECUTABLE( Parameterization itkQuadEdgeMeshLinearParameterizationTest.cxx )
TARGET_LINK_LIBRARIES( Parameterization ITKCommon ITKQuadEdgeMesh )

FOREACH( border 0 1 )
  FOREACH( coeff 0 1 2 3 4 )
    ADD_TEST( itkQuadEdgeMeshLinearParameterizationTest${border}_${coeff}
    Parameterization
    ${DATAROOT}/mushroom.vtk ${border} ${coeff}
    param_border${border}_coeff${coeff}.vtk
    )
  ENDFOREACH( coeff )
ENDFOREACH( border )
