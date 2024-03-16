add_library(CsScript SHARED "")
add_library(CopperSpice::CsScript ALIAS CsScript)

set_target_properties(CsScript PROPERTIES OUTPUT_NAME CsScript${BUILD_ABI})

add_definitions(
    -DQLALR_NO_SCRIPTGRAMMAR_DEBUG_INFO
    -DJS_NO_EXPORT
    -DQLALR_NO_QSCRIPTGRAMMAR_DEBUG_INFO
    -DWTF_USE_JAVASCRIPTCORE_BINDINGS=1
    -DWTF_CHANGES=1
    -DBUILDING_CS=1
    -DWTF_USE_ACCELERATED_COMPOSITING
    -DJSC=QTJSC
    -Djscyyparse=qtjscyyparse
    -Djscyylex=qtjscyylex
    -Djscyyerror=qtjscyyerror
    -DWTF=QTWTF
    -DUSE_SYSTEM_MALLOC
    -DQT_BUILD_SCRIPT_LIB
)

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")

   if (CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL "7.1.0")
      set_target_properties(CsScript
         PROPERTIES
         COMPILE_OPTIONS -Wno-expansion-to-defined
      )
   endif()

endif()

set(EXTRA_SCRIPT_CXXFLAGS)
set(EXTRA_SCRIPT_LDFLAGS)

target_include_directories(CsScript
   PUBLIC
   $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/include/QtScript>
   $<INSTALL_INTERFACE:include/QtScript>
)

include_directories(
    ${CMAKE_BINARY_DIR}/privateinclude/QtCore/private
    ${CMAKE_BINARY_DIR}/privateinclude/QtScript/private

    ${CMAKE_CURRENT_SOURCE_DIR}/api
    ${CMAKE_CURRENT_SOURCE_DIR}/bridge
    ${CMAKE_CURRENT_SOURCE_DIR}/parser

    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/heap
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/unicode
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders
    ${PROJECT_SOURCE_DIR}src/3rdparty/javascriptcore/JavaScriptCore/wtf/qt
)

include(api/api.cmake)
include(bridge/bridge.cmake)
include(parser/parser.cmake)

list(APPEND JSCORE_SCRIPT_INCLUDES
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/CodeBlock.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/Opcode.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/SamplingTool.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/JumpTable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/Instruction.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/StructureStubInfo.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/EvalCodeCache.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre_internal.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/ucpinternal.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/config.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/JavaScriptCorePrefix.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/OpaqueJSString.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSStringRefBSTR.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JavaScript.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSValueRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackFunction.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/APICast.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSObjectRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSBasePrivate.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSContextRefPrivate.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSContextRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSStringRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/WebKitAvailability.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSClassRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSBase.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/APIShims.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSStringRefCF.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSProfilerPrivate.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSRetainPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackObjectFunctions.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JavaScriptCore.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/Interpreter.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/CallFrameClosure.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/RegisterFile.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/Register.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/CachedCall.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/CallFrame.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/ArrayPrototype.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/RegExpConstructor.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/RegExpObject.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/Grammar.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/GeneratedJITStubs_RVCT.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/MathObject.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/JSONObject.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/Lexer.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/NumberConstructor.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/StringPrototype.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/DatePrototype.lut.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/WRECParser.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/CharacterClassConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/Escapes.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/CharacterClass.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/Quantifier.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/WRECFunctors.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/WRECGenerator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wrec/WREC.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITInlineMethods.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/ExecutableAllocator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITStubs.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITCode.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITStubCall.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JIT.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/OpaqueJSString.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JavaScript.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSValueRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/APICast.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSObjectRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSContextRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSStringRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/WebKitAvailability.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSBase.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSStringRefCF.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JSRetainPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/ForwardingHeaders/JavaScriptCore/JavaScriptCore.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger/Debugger.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger/DebuggerActivation.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger/DebuggerCallFrame.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler/Label.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler/LabelScope.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler/RegisterID.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler/BytecodeGenerator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/CodeLocation.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/ARMAssembler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerCodeRef.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/AssemblerBuffer.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerX86_64.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/ARMv7Assembler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/LinkBuffer.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/RepatchBuffer.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerARM.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerX86Common.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerARMv7.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssembler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/AssemblerBufferWithConstantPool.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/AbstractMacroAssembler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerX86.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/X86Assembler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ClassInfo.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ArgList.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/TimeoutChecker.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumberConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSGlobalData.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Identifier.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BooleanPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateConversion.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSZombie.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/WeakRandom.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/FunctionConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/CollectorHeapIterator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ScopeChainMark.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSGlobalObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BatchedTransitionOptimizer.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSVariableObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertyDescriptor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BooleanConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Completion.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Structure.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/SymbolTable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/MarkStack.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ErrorPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSType.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DatePrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ObjectConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSArray.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumericStrings.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSActivation.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSImmediate.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSONObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ObjectPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NativeErrorPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ArrayConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSValue.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateInstance.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSAPIValueWrapper.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NativeFunctionWrapper.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSGlobalObjectFunctions.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpMatchesArray.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSByteArray.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Error.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/InitializeThreading.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Collector.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ArrayPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertyMapHashTable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSTypeInfo.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StructureTransitionTable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumberPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Tracing.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/UStringImpl.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/GetterSetter.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumberObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Executable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSPropertyNameIterator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Lookup.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/UString.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/MathObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/SmallStrings.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/FunctionPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/InternalFunction.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpPrototype.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BooleanObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Protect.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Arguments.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StructureChain.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSFunction.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/WeakGCMap.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSLock.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertySlot.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSNumberCell.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Operations.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NativeErrorConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ConstructData.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringObjectThatMasqueradesAsUndefined.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ScopeChain.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ErrorInstance.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/LiteralParser.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSCell.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ExceptionHelpers.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PrototypeFunction.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/WeakGCPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertyNameArray.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSString.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/GlobalEvalFunction.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateInstanceCache.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringBuilder.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PutPropertySlot.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ErrorConstructor.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSStaticScopeObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSWrapperObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/CommonIdentifiers.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSNotAnObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExp.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/CallData.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringObject.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/SourceProvider.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/ResultType.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/ParserArena.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/Nodes.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/NodeInfo.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/Lexer.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/SourceCode.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/NodeConstructors.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/Parser.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ThreadSpecific.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RefCountedLeakCounter.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/PassRefPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashFunctions.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashMap.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashIterators.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/SegmentedVector.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Vector.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/TypeTraits.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/TCPackedCache.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ASCIICType.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/android/AndroidThreading.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/dtoa.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RefPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/TCSpinLock.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/OwnArrayPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Platform.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/PossiblyNull.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/MessageQueue.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Forward.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/TCSystemAlloc.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/StringHashFunctions.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/CurrentTime.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/MainThread.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/FastAllocBase.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/CrossThreadRefCounted.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/MathExtras.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Threading.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/StdLibExtras.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RandomNumberSeed.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RandomNumber.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/DateMath.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/PtrAndFlags.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/GetPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ByteArray.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/OwnPtrCommon.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ListRefPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashTraits.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/VectorTraits.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/AVLTree.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Assertions.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode/Collator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode/qt4/UnicodeQt4.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode/UTF8.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode/Unicode.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Locker.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/StringExtras.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/DisallowCType.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/FastMalloc.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RefCounted.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashTable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/MallocZoneSupport.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/OwnPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Deque.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Noncopyable.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/TCPageMap.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashSet.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RefPtrHashMap.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ThreadIdentifierDataPthreads.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/PassOwnPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/NotFound.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/VMTags.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RetainPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/OwnFastMallocPtr.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/AlwaysInline.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashCountedSet.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/UnusedParam.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ListHashSet.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexJIT.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexInterpreter.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexPattern.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexParser.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexCompiler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/Profiler.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/Profile.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/ProfileGenerator.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/ProfileNode.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/ProfilerServer.h
    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/CallIdentifier.h

    # https://bugreports.qt.io/browse/QTBUG-9958
    # ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/chartables.c

    ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/ucptable.cpp
)

target_sources(CsScript
   PRIVATE
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSBase.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackFunction.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSCallbackObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSClassRef.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSContextRef.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSObjectRef.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSStringRef.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/JSValueRef.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/API/OpaqueJSString.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/ARMAssembler.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/assembler/MacroAssemblerARM.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/CodeBlock.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/JumpTable.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/Opcode.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/SamplingTool.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecode/StructureStubInfo.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler/BytecodeGenerator.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/bytecompiler/NodesCodegen.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger/DebuggerActivation.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger/DebuggerCallFrame.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/debugger/Debugger.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/CallFrame.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/Interpreter.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/interpreter/RegisterFile.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/ExecutableAllocatorFixedVMPool.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/ExecutableAllocatorPosix.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/ExecutableAllocatorWin.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/ExecutableAllocator.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITArithmetic.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITCall.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JIT.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITOpcodes.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITPropertyAccess.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/jit/JITStubs.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/Lexer.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/Nodes.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/ParserArena.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/parser/Parser.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/Profile.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/ProfileGenerator.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/ProfileNode.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/profiler/Profiler.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ArgList.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Arguments.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ArrayConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ArrayPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BooleanConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BooleanObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/BooleanPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/CallData.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Collector.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/CommonIdentifiers.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Completion.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ConstructData.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateConversion.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DateInstance.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/DatePrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ErrorConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Error.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ErrorInstance.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ErrorPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ExceptionHelpers.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Executable.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/FunctionConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/FunctionPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/GetterSetter.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/GlobalEvalFunction.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Identifier.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/InitializeThreading.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/InternalFunction.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSActivation.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSAPIValueWrapper.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSArray.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSByteArray.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSCell.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSFunction.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSGlobalData.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSGlobalObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSGlobalObjectFunctions.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSImmediate.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSLock.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSNotAnObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSNumberCell.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSONObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSPropertyNameIterator.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSStaticScopeObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSString.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSValue.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSVariableObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/JSWrapperObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/LiteralParser.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Lookup.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/MarkStackPosix.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/MarkStackWin.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/MarkStack.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/MathObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NativeErrorConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NativeErrorPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumberConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumberObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/NumberPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ObjectConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ObjectPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Operations.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertyDescriptor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertyNameArray.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PropertySlot.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/PrototypeFunction.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExp.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/RegExpPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/ScopeChain.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/SmallStrings.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringConstructor.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringObject.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StringPrototype.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/StructureChain.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/Structure.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/TimeoutChecker.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/UString.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/runtime/UStringImpl.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Assertions.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ByteArray.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/CurrentTime.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/DateMath.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/dtoa.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/FastMalloc.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/HashTable.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/MainThread.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/qt/MainThreadQt.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/qt/ThreadingQt.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RandomNumber.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/RefCountedLeakCounter.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/ThreadingNone.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Threading.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/TypeTraits.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode/CollatorDefault.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/wtf/unicode/UTF8.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexCompiler.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexInterpreter.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/yarr/RegexJIT.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/generated/Grammar.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre_compile.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre_exec.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre_tables.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre_ucp_searchfuncs.cpp
   ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/pcre/pcre_xclass.cpp
)

macro_generate_public("${SCRIPT_PUBLIC_INCLUDES}"   QtScript)
macro_generate_private("${SCRIPT_PRIVATE_INCLUDES}" QtScript)
macro_generate_misc("${SCRIPT_INCLUDES}"            QtScript)

if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
   add_definitions(-DENABLE_JSC_MULTIPLE_THREADS=0)

   list(APPEND EXTRA_SCRIPT_LDFLAGS
      -framework
      CoreFoundation
   )

elseif(CMAKE_SYSTEM_NAME MATCHES "Windows")
   target_link_libraries(CsScript
      PRIVATE
      winmm
   )

   if(NOT MSVC)
      list(APPEND EXTRA_SCRIPT_LDFLAGS
         -Wl,-s
      )
   endif()

   list(APPEND SCRIPT_INCLUDES
      ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/os-win32/stdbool.h
      ${PROJECT_SOURCE_DIR}/src/3rdparty/javascriptcore/JavaScriptCore/os-win32/stdint.h
   )

endif()

function_variable_fixup("${EXTRA_SCRIPT_CXXFLAGS}" EXTRA_SCRIPT_CXXFLAGS)
function_variable_fixup("${EXTRA_SCRIPT_LDFLAGS}"  EXTRA_SCRIPT_LDFLAGS)

target_sources(CsScript
   PRIVATE
   ${SCRIPT_INCLUDES}
)

target_link_libraries(CsScript
   PRIVATE
   CsCore
)

set_target_properties(CsScript
   PROPERTIES
   COMPILE_FLAGS ${EXTRA_SCRIPT_CXXFLAGS}
   LINK_FLAGS ${EXTRA_SCRIPT_LDFLAGS}
)

function_generate_resources(CsScript)

macro_generate_package(
   CsScript
   QtScript
   "${EXTRA_SCRIPT_CXXFLAGS}"
   "CsCore"
)

install(
   TARGETS CsScript
   EXPORT CopperSpiceLibraryTargets ${INSTALL_TARGETS_DEFAULT_ARGS}
   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(
   DIRECTORY ${CMAKE_BINARY_DIR}/include/QtScript
   DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
   COMPONENT Devel
)
