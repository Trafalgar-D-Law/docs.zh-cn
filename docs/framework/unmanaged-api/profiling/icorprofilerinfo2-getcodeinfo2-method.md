---
title: "ICorProfilerInfo2::GetCodeInfo2 方法"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorProfilerInfo2.GetCodeInfo2
api_location: mscorwks.dll
api_type: COM
f1_keywords: ICorProfilerInfo2::GetCodeInfo2
helpviewer_keywords:
- ICorProfilerInfo2::GetCodeInfo2 method [.NET Framework profiling]
- GetCodeInfo2 method [.NET Framework profiling]
ms.assetid: 532da6ee-7f0a-401b-a61e-fc47ec235d2e
topic_type: apiref
caps.latest.revision: "17"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 6c2526c286e4014b32e63ec34f9d212a5f657756
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="icorprofilerinfo2getcodeinfo2-method"></a><span data-ttu-id="0ca9d-102">ICorProfilerInfo2::GetCodeInfo2 方法</span><span class="sxs-lookup"><span data-stu-id="0ca9d-102">ICorProfilerInfo2::GetCodeInfo2 Method</span></span>
<span data-ttu-id="0ca9d-103">获取与指定 `FunctionID` 关联的本机代码的范围。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-103">Gets the extents of native code associated with the specified `FunctionID`.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="0ca9d-104">语法</span><span class="sxs-lookup"><span data-stu-id="0ca9d-104">Syntax</span></span>  
  
```  
HRESULT GetCodeInfo2(  
    [in]  FunctionID functionID,  
    [in]  ULONG32 cCodeInfos,  
    [out] ULONG32 *pcCodeInfos,  
    [out, size_is(cCodeInfos), length_is(*pcCodeInfos)]  
    COR_PRF_CODE_INFO codeInfos[]);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="0ca9d-105">参数</span><span class="sxs-lookup"><span data-stu-id="0ca9d-105">Parameters</span></span>  
 `functionID`  
 <span data-ttu-id="0ca9d-106">[in] 与本机代码关联的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-106">[in] The ID of the function with which the native code is associated.</span></span>  
  
 `cCodeInfos`  
 <span data-ttu-id="0ca9d-107">[in] `codeInfos` 数组的大小。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-107">[in] The size of the `codeInfos` array.</span></span>  
  
 `pcCodeInfos`  
 <span data-ttu-id="0ca9d-108">[out]指向的总数的指针[COR_PRF_CODE_INFO](../../../../docs/framework/unmanaged-api/profiling/cor-prf-code-info-structure.md)结构可用。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-108">[out] A pointer to the total number of [COR_PRF_CODE_INFO](../../../../docs/framework/unmanaged-api/profiling/cor-prf-code-info-structure.md) structures available.</span></span>  
  
 `codeInfos`  
 <span data-ttu-id="0ca9d-109">[out] 调用方提供的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-109">[out] A caller-provided buffer.</span></span> <span data-ttu-id="0ca9d-110">返回此方法后，它包含一个 `COR_PRF_CODE_INFO` 结构数组，每个结构描述一个本机代码块。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-110">After the method returns, it contains an array of `COR_PRF_CODE_INFO` structures, each of which describes a block of native code.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="0ca9d-111">备注</span><span class="sxs-lookup"><span data-stu-id="0ca9d-111">Remarks</span></span>  
 <span data-ttu-id="0ca9d-112">范围按 Microsoft 中间语言 (MSIL) 偏移递增的顺序进行排序。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-112">The extents are sorted in order of increasing Microsoft intermediate language (MSIL) offset.</span></span>  
  
 <span data-ttu-id="0ca9d-113">返回 `GetCodeInfo2` 后，必须验证 `codeInfos` 缓冲区大小是否足以包含所有 `COR_PRF_CODE_INFO` 结构。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-113">After `GetCodeInfo2` returns, you must verify that the `codeInfos` buffer was large enough to contain all the `COR_PRF_CODE_INFO` structures.</span></span> <span data-ttu-id="0ca9d-114">为此，请将 `cCodeInfos` 的值和 `cchName` 参数的值进行比较。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-114">To do this, compare the value of `cCodeInfos` with the value of the `cchName` parameter.</span></span> <span data-ttu-id="0ca9d-115">如果由 `COR_PRF_CODE_INFO` 结构大小划分的 `cCodeInfos` 值小于 `pcCodeInfos` 值，请分配更大的 `codeInfos` 缓冲区，将 `cCodeInfos` 更新为新的更大大小，并再次调用 `GetCodeInfo2`。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-115">If `cCodeInfos` divided by the size of a `COR_PRF_CODE_INFO` structure is smaller than `pcCodeInfos`, allocate a larger `codeInfos` buffer, update `cCodeInfos` with the new, larger size, and call `GetCodeInfo2` again.</span></span>  
  
 <span data-ttu-id="0ca9d-116">或者，可以先用长度为零的 `codeInfos` 缓冲区调用 `GetCodeInfo2` 以获取正确的缓冲区大小。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-116">Alternatively, you can first call `GetCodeInfo2` with a zero-length `codeInfos` buffer to obtain the correct buffer size.</span></span> <span data-ttu-id="0ca9d-117">然后可将 `codeInfos` 缓冲区大小设置为 `pcCodeInfos` 中返回的，乘以 `COR_PRF_CODE_INFO` 结构大小的值，并再次调用 `GetCodeInfo2`。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-117">You can then set the `codeInfos` buffer size to the value returned in `pcCodeInfos`, multiplied by the size of a `COR_PRF_CODE_INFO` structure, and call `GetCodeInfo2` again.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="0ca9d-118">要求</span><span class="sxs-lookup"><span data-stu-id="0ca9d-118">Requirements</span></span>  
 <span data-ttu-id="0ca9d-119">**平台：**请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="0ca9d-119">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="0ca9d-120">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="0ca9d-120">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="0ca9d-121">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="0ca9d-121">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="0ca9d-122">**.NET framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="0ca9d-122">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="0ca9d-123">另请参阅</span><span class="sxs-lookup"><span data-stu-id="0ca9d-123">See Also</span></span>  
 [<span data-ttu-id="0ca9d-124">GetCodeInfo3 方法</span><span class="sxs-lookup"><span data-stu-id="0ca9d-124">GetCodeInfo3 Method</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo4-getcodeinfo3-method.md)  
 [<span data-ttu-id="0ca9d-125">ICorProfilerInfo2 接口</span><span class="sxs-lookup"><span data-stu-id="0ca9d-125">ICorProfilerInfo2 Interface</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo2-interface.md)  
 [<span data-ttu-id="0ca9d-126">分析接口</span><span class="sxs-lookup"><span data-stu-id="0ca9d-126">Profiling Interfaces</span></span>](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)  
 [<span data-ttu-id="0ca9d-127">分析</span><span class="sxs-lookup"><span data-stu-id="0ca9d-127">Profiling</span></span>](../../../../docs/framework/unmanaged-api/profiling/index.md)