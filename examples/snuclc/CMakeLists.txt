# If we don't need RTTI or EH, there's no reason to export anything
# from the plugin.
if( NOT MSVC ) # MSVC mangles symbols differently, and
                # PrintFunctionNames.export contains C++ symbols.
  if( NOT LLVM_REQUIRES_RTTI )
    if( NOT LLVM_REQUIRES_EH )
      set(LLVM_EXPORTED_SYMBOL_FILE ${CMAKE_CURRENT_SOURCE_DIR}/SnuCLTranslator.exports)
    endif()
  endif()
endif()

add_definitions(-D__SNUCL_COMPILER__)

add_llvm_loadable_module(SnuCLTranslator 
	BarrierMarker.cpp
	FunctionAnalyzer.cpp
	TransformBasic.cpp
	BitVector.cpp
	FunctionDuplicator.cpp
	TransformCoalescing.cpp
	CLDeclPrinter.cpp
	FunctionInliner.cpp
	TransformFlowKeyword.cpp
	CLExpressions.cpp
	IdiomDuplicator.cpp
	TransformLICM.cpp
	CLStmtPrinter.cpp
	PostDominator.cpp
	TransformLocalVD.cpp
	CLTranslator.cpp
	ReachingDefinitions.cpp
	TransformVE.cpp
	CLUtils.cpp
	SnuCLTranslator.cpp
	TransformVector.cpp
	Dominator.cpp
	StmtSimplifier.cpp
	TransformWCL.cpp
	DUChains.cpp
	StmtTransformer.cpp
	TransformWCR.cpp)

if(LLVM_ENABLE_PLUGINS AND (WIN32 OR CYGWIN))
  target_link_libraries(SnuCLTranslator ${cmake_2_8_12_PRIVATE}
    clangAST
    clangBasic
    clangFrontend
    LLVMSupport
    )
endif()
