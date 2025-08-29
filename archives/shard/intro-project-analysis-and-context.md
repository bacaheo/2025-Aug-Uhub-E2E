# Intro Project Analysis and Context

## Existing Project Overview

**Analysis Source:** Tài liệu project brief từ Business Analyst + IDE-based analysis

**Current Project State:**  
UHub hiện đang là nền tảng **shopper journey** cho Unilever Việt Nam bao gồm:
- Tích hợp sẵn với PG App để phân phối quà tặng
- Quy trình quản lý quà tặng thủ công bằng Excel  
- Khả năng báo cáo cơ bản
- Key Account ecosystem với 8 nhà bán lẻ lớn, hơn 500 stores

## Available Documentation Analysis

Từ tóm tắt dự án và cấu trúc file hiện tại:
- ✅ **Tech Stack Documentation** - Từ brief: Angular, .NET Core, Azure
- ✅ **Integration Documentation** - Có sẵn thông số kỹ thuật tích hợp UGMS  
- ✅ **Business Process Documentation** - So sánh quy trình hiện tại và đề xuất
- ❌ **Technical Architecture Details** - Cần phân tích sâu hơn
- ❌ **UX/UI Guidelines** - Cho mobile webapp design

## Enhancement Scope Definition

**Enhancement Type:** 
- ✅ **Integration with New Systems** (UGMS-UHub API)
- ✅ **New Feature Addition** (E2E gift tracking)
- ✅ **Major Feature Modification** (Transform manual → automated)

**Enhancement Description:**  
Tạo hệ thống tự động hóa hoàn chỉnh cho quản lý quà tặng vật lý, tích hợp UGMS với UHub để thay thế quy trình Excel thủ công bằng xác nhận số hóa (digital confirmation), theo dõi thời gian thực (real-time tracking), và đối soát tự động (automatic reconciliation).

**Impact Assessment:**
- ✅ **Major Impact** - Yêu cầu thay đổi kiến trúc, tích hợp API mới, và nhiều quy trình người dùng mới

## Goals and Background Context

**Goals:**
- Tự động hóa 80% quy trình quản lý quà tặng (tăng từ 20% lên 80%)
- Giảm 90% thời gian đối soát thủ công 
- Tạo khả năng giám sát từ đầu đến cuối (end-to-end visibility) từ trung tâm phân phối đến người tiêu dùng
- Giảm 60% lãng phí quà tặng thông qua quy trình thu hồi có hệ thống

**Background Context:**  
Unilever Việt Nam hiện tại đang vận hành 15-25 chiến dịch quà tặng mỗi năm với ngân sách từ 500K-2M USD cho mỗi chiến dịch. Quy trình thủ công hiện tại đang tạo ra 4 điểm khó khăn chính: không có sự phối hợp trong việc thu hồi quà tặng, không có kiểm soát hệ thống, không có thanh lý tại cửa hàng, và đối soát thủ công. Hệ thống E2E này sẽ tích hợp với UHub shopper journey hiện có để tạo ra chuyển đổi số hoàn chỉnh.

## Change Log

| Change | Date | Version | Description | Author |
|--------|------|---------|-------------|---------|
| Initial PRD Creation | 2025-08-29 | 1.0 | Complete brownfield PRD for E2E Gift Management System | John (PM) |
