<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DT 역량 진단 및 AI 활용 직무교육 수요조사</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
            background-image: url('https://images.unsplash.com/photo-1531297484001-80022131f5a1?q=80&w=2020&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .progress-bar-fill { transition: width 0.5s ease-in-out; }
        .fade-in { animation: fadeIn 0.5s ease-in-out; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        input[type="radio"]:focus-visible + label,
        input[type="checkbox"]:focus-visible + label {
            outline: 2px solid #3b82f6; outline-offset: 2px; border-radius: 0.25rem;
        }
        .result-card {
            background-color: white; border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
            padding: 1.5rem; margin-bottom: 1.5rem;
        }
        .chat-bubble {
            max-width: 80%;
            padding: 10px 15px;
            border-radius: 20px;
            word-wrap: break-word;
        }
        .user-bubble { background-color: #3b82f6; color: white; align-self: flex-end; }
        .bot-bubble { background-color: #e5e7eb; color: #1f2937; align-self: flex-start; }
        .loader {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3498db;
            border-radius: 50%;
            width: 25px;
            height: 25px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .suggestion-chip {
            background-color: #dbeafe;
            color: #1e40af;
            border: 1px solid #93c5fd;
            padding: 6px 12px;
            border-radius: 9999px;
            font-size: 0.875rem;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .suggestion-chip:hover {
            background-color: #bfdbfe;
        }
    </style>
</head>
<body class="text-gray-800">

    <div class="container mx-auto max-w-4xl p-4 sm:p-8">
        
        <!-- Start View -->
        <div id="startContainer" class="fade-in min-h-[80vh] flex items-center justify-center">
            <div class="text-center bg-white/80 backdrop-blur-md p-8 sm:p-12 rounded-xl shadow-2xl">
                 <header class="mb-8">
                    <h1 class="text-4xl font-bold text-gray-900">DT 역량 진단</h1>
                    <p class="mt-4 text-lg text-gray-700">
                        본 진단은 임직원 여러분의 디지털 전환(DT) 역량 수준을 파악하고,<br>
                        효과적인 AI 직무교육 과정을 설계하기 위해 실시됩니다.
                    </p>
                    <p class="mt-2 text-md text-gray-600">잠시 시간을 내어 성실하게 답변해주시면 감사하겠습니다.</p>
                </header>
                <div>
                    <button id="startSurveyBtn" class="bg-blue-600 text-white font-bold py-4 px-10 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-300 transition duration-300 text-xl shadow-lg hover:shadow-xl transform hover:-translate-y-1">
                        진단 시작하기
                    </button>
                </div>
            </div>
        </div>

        <!-- Survey View (Initially hidden) -->
        <div id="surveyContainer" class="hidden">
            <header class="text-center mb-8 bg-white/80 backdrop-blur-md p-6 rounded-xl shadow-lg">
                <h1 class="text-3xl font-bold text-gray-900">DT 역량 진단 및 AI 활용 직무교육 수요조사</h1>
            </header>

            <div class="w-full bg-gray-200 rounded-full h-2.5 mb-8">
                <div id="progressBar" class="bg-blue-600 h-2.5 rounded-full progress-bar-fill" style="width: 0%"></div>
            </div>

            <form id="surveyForm" class="space-y-10">
                <div id="question1" class="bg-white p-6 rounded-lg shadow-md fade-in">
                    <fieldset>
                        <legend class="text-lg font-semibold text-gray-900 mb-4">1. 귀하의 소속 부서를 선택해주세요. <span class="text-red-500">*</span></legend>
                        <select name="department" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500" required>
                            <option value="">부서를 선택하세요</option>
                            <option value="경영기획그룹">경영기획그룹</option>
                            <option value="사업그룹">사업그룹</option>
                            <option value="시설그룹">시설그룹</option>
                            <option value="운영그룹">운영그룹</option>
                            <option value="기타">기타</option>
                        </select>
                    </fieldset>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-md fade-in">
                    <h2 class="text-xl font-bold mb-6 text-center text-blue-700">Part 1. DT 현황 및 역량 진단</h2>
                    <fieldset id="question2" class="mb-8">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">2. [현황 및 필요성] 귀하의 직무에 현재 AI, 자동화 등 디지털 기술(DT)이 어느정도 도입되어 있으며, 향후 도입 필요성은 어느 수준이라고 판단하십니까? <span class="text-red-500">*</span></legend>
                        <div class="space-y-3">
                            <div class="flex items-center"><input type="radio" id="q2_1" name="dt_status" value="4" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500" required><label for="q2_1" class="ml-3 block text-gray-700">현재 DT가 활발히 도입되어 있으며, 현 수준 유지 또는 고도화가 필요한 상황</label></div>
                            <div class="flex items-center"><input type="radio" id="q2_2" name="dt_status" value="3" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q2_2" class="ml-3 block text-gray-700">일부 DT가 도입되었으나, 추가적인 확대 및 개선이 필요한 상황</label></div>
                            <div class="flex items-center"><input type="radio" id="q2_3" name="dt_status" value="2" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q2_3" class="ml-3 block text-gray-700">현재 DT는 도입되지 않았으나, 업무 효율화 등을 위해 시급히 도입이 필요한 상황</label></div>
                            <div class="flex items-center"><input type="radio" id="q2_4" name="dt_status" value="1" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q2_4" class="ml-3 block text-gray-700">DT 도입 필요성을 느끼지 못하거나, 현재 업무 방식에 만족하는 상황</label></div>
                        </div>
                    </fieldset>
                    <fieldset id="question3" class="mb-8">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">3. [업무 프로세스] 귀하의 핵심 업무 프로세스는 어느 정도로 디지털화 되어 있습니까? <span class="text-red-500">*</span></legend>
                        <div class="space-y-3">
                            <div class="flex items-center"><input type="radio" id="q3_1" name="process_digital" value="4" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500" required><label for="q3_1" class="ml-3 block text-gray-700">업무의 모든 단계가 완전 자동화 또는 디지털화된 시스템 내에서 처리됨</label></div>
                            <div class="flex items-center"><input type="radio" id="q3_2" name="process_digital" value="3" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q3_2" class="ml-3 block text-gray-700">업무의 특정 주요 단계가 ERP, 그룹웨어 등 디지털화된 시스템 내에서 처리됨</label></div>
                            <div class="flex items-center"><input type="radio" id="q3_3" name="process_digital" value="2" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q3_3" class="ml-3 block text-gray-700">대부분의 업무를 수기 또는 개별 문서(엑셀, 워드 등)로 처리하며, 시스템은 보조적으로 사용</label></div>
                            <div class="flex items-center"><input type="radio" id="q3_4" name="process_digital" value="1" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q3_4" class="ml-3 block text-gray-700">모든 업무를 수기 또는 오프라인으로 처리</label></div>
                        </div>
                    </fieldset>
                    <fieldset id="question4" class="mb-8">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">4. [DT 적극성] 귀하는 업무를 수행하면서 여러가지 디지털 실험과 시도를 도전하고 계십니까? <span class="text-red-500">*</span></legend>
                         <div class="flex justify-between items-center space-x-2">
                            <span class="text-sm text-gray-600">전혀<br>아니다</span>
                            <div class="flex items-center"><label for="q4_1" class="text-center">1</label><input type="radio" id="q4_1" name="dt_proactive" value="1" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500 mx-2" required></div>
                            <div class="flex items-center"><label for="q4_2" class="text-center">2</label><input type="radio" id="q4_2" name="dt_proactive" value="2" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500 mx-2"></div>
                            <div class="flex items-center"><label for="q4_3" class="text-center">3</label><input type="radio" id="q4_3" name="dt_proactive" value="3" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500 mx-2"></div>
                            <div class="flex items-center"><label for="q4_4" class="text-center">4</label><input type="radio" id="q4_4" name="dt_proactive" value="4" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500 mx-2"></div>
                            <div class="flex items-center"><label for="q4_5" class="text-center">5</label><input type="radio" id="q4_5" name="dt_proactive" value="5" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500 mx-2"></div>
                            <span class="text-sm text-gray-600">매우<br>그렇다</span>
                        </div>
                    </fieldset>
                     <fieldset id="question5" class="mb-8">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">5. [AI 이해 수준] AI 기술이 조직의 업무 또는 비즈니스에 어떻게 기여할 수 있는 지에 대한 본인의 이해 수준은 어느 정도입니까? <span class="text-red-500">*</span></legend>
                        <div class="space-y-3">
                            <div class="flex items-center"><input type="radio" id="q5_1" name="ai_understanding" value="4" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500" required><label for="q5_1" class="ml-3 block text-gray-700">AI 기술의 비즈니스 적용 전략을 수립하고, 구체적인 기여 방안을 제시할 수 있다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q5_2" name="ai_understanding" value="3" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q5_2" class="ml-3 block text-gray-700">AI 기술의 종류와 특징을 이해하고, 우리 조직의 문제 해결에 적용할 방안을 모색할 수 있다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q5_3" name="ai_understanding" value="2" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q5_3" class="ml-3 block text-gray-700">AI의 일반적인 개념은 알고 있으나, 실제 업무 적용방식에 대해서는 잘 모른다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q5_4" name="ai_understanding" value="1" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q5_4" class="ml-3 block text-gray-700">AI에 대해 거의 알지 못한다.</label></div>
                        </div>
                    </fieldset>
                    <fieldset id="question6" class="mb-8">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">6. [데이터 분석 경험] 데이터를 수집/가공하고 분석 및 시각화 도구를 통해 업무에 활용해본 경험은 어느 정도입니까? <span class="text-red-500">*</span></legend>
                         <div class="space-y-3">
                            <div class="flex items-center"><input type="radio" id="q6_1" name="data_experience" value="4" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500" required><label for="q6_1" class="ml-3 block text-gray-700">다양한 데이터를 직접 수집/가공하고, BI 툴 등을 활용해 시각화 및 인사이트 도출이 가능하다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q6_2" name="data_experience" value="3" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q6_2" class="ml-3 block text-gray-700">엑셀 등 기본적인 툴을 사용하여 데이터를 정제하고, 차트/그래프 등으로 시각화할 수 있다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q6_3" name="data_experience" value="2" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q6_3" class="ml-3 block text-gray-700">주어진 데이터를 간단히 확인하거나, 정해진 양식에 따라 정리하는 수준이다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q6_4" name="data_experience" value="1" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q6_4" class="ml-3 block text-gray-700">데이터 분석 및 시각화 도구를 사용해본 경험이 없다.</label></div>
                        </div>
                    </fieldset>
                     <fieldset id="question7">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">7. [프로그래밍 경험] 업무 자동화 또는 데이터 분석을 위해 프로그래밍 언어(Python, SQL 등)를 활용해보신 경험이 있으십니까? <span class="text-red-500">*</span></legend>
                         <div class="space-y-3">
                            <div class="flex items-center"><input type="radio" id="q7_1" name="programming_experience" value="4" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500" required><label for="q7_1" class="ml-3 block text-gray-700">프로그래밍 언어를 능숙하게 사용하여 업무 자동화 또는 데이터 분석을 수행한다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q7_2" name="programming_experience" value="3" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q7_2" class="ml-3 block text-gray-700">간단한 코드(SQL 쿼리, Python 스크립트 등)를 작성하거나 수정하여 업무에 활용해본 경험이 있다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q7_3" name="programming_experience" value="2" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q7_3" class="ml-3 block text-gray-700">다른 사람이 작성한 코드를 읽고 이해하는 정도는 가능하다.</label></div>
                            <div class="flex items-center"><input type="radio" id="q7_4" name="programming_experience" value="1" class="h-4 w-4 text-blue-600 border-gray-300 focus:ring-blue-500"><label for="q7_4" class="ml-3 block text-gray-700">프로그래밍 언어를 사용해본 경험이 없다.</label></div>
                        </div>
                    </fieldset>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-md fade-in">
                     <h2 class="text-xl font-bold mb-6 text-center text-blue-700">Part 2. AI 직무교육 수요조사</h2>
                    <fieldset id="question8" class="mb-8">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">8. [DT 필요 영역] 귀하의 직무(또는 부서)에서 향후 DT 기술이 더욱 적극적으로 활용되어야 한다고 생각하는 업무 영역은 무엇입니까? (모두 선택) <span class="text-red-500">*</span></legend>
                        <div class="space-y-3" id="q8_container">
                            <div class="flex items-center"><input type="checkbox" id="q8_1" name="dt_need_area" value="행정업무 자동화" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q8_1" class="ml-3 block text-gray-700">반복적 행정업무 (문서 작성, 자료 수집, 표/보고서 작성, 수기 입력 등)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q8_2" name="dt_need_area" value="데이터 분석/시각화" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q8_2" class="ml-3 block text-gray-700">데이터 분석 및 시각화 (엑셀, BI 툴 등으로 매출, 고객, 내부 데이터 가공/해석/시각화 등)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q8_3" name="dt_need_area" value="협업/소통 관리" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q8_3" class="ml-3 block text-gray-700">커뮤니케이션 및 협업 관리 (회의록 작성, 이메일/보고 자동화, 일정 공유 및 업무 협업 등)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q8_4" name="dt_need_area" value="고객 관리/마케팅" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q8_4" class="ml-3 block text-gray-700">고객 관리 및 마케팅 (고객 데이터 분석, 타겟 마케팅, 챗봇 상담 등)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q8_5" name="dt_need_area" value="기타" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q8_5" class="ml-3 block text-gray-700">기타</label></div>
                        </div>
                    </fieldset>
                    <fieldset id="question9">
                        <legend class="text-lg font-semibold text-gray-900 mb-4">9. 수요조사 결과를 바탕으로, 9~10월 중 DT/AI 직무교육을 개설하고자 합니다. 교육 참여 희망 시, 다음 중 희망 과정을 모두 선택해주세요. <span class="text-red-500">*</span></legend>
                        <div class="space-y-3" id="q9_container">
                            <div class="flex items-center"><input type="checkbox" id="q9_1" name="training_course" value="[기본] AI 리터러시" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q9_1" class="ml-3 block text-gray-700"><b>[기본] AI 리터러시</b> (AI 개념, 최신 트렌드, 비즈니스 활용 사례)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q9_2" name="training_course" value="[기본] 데이터 리터러시" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q9_2" class="ml-3 block text-gray-700"><b>[기본] 데이터 리터러시</b> (데이터 분석 기초, 엑셀/BI 툴 활용)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q9_3" name="training_course" value="[심화] 업무 자동화 실습" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q9_3" class="ml-3 block text-gray-700"><b>[심화] 업무 자동화 실습</b> (RPA, 노코드 툴을 활용한 자동화)</label></div>
                            <div class="flex items-center"><input type="checkbox" id="q9_4" name="training_course" value="[심화] Python 데이터 분석" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q9_4" class="ml-3 block text-gray-700"><b>[심화] Python 데이터 분석</b> (Python 기초, 데이터 분석 라이브러리 활용)</label></div>
                             <div class="flex items-center"><input type="checkbox" id="q9_5" name="training_course" value="희망 과정 없음" class="h-4 w-4 text-blue-600 border-gray-300 rounded focus:ring-blue-500"><label for="q9_5" class="ml-3 block text-gray-700">희망 과정 없음</label></div>
                        </div>
                    </fieldset>
                </div>

                <div class="text-center">
                    <button type="submit" class="bg-blue-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-300 transition duration-300">
                        결과 분석하기
                    </button>
                </div>
            </form>
        </div>

        <!-- Result View (Initially hidden) -->
        <div id="resultContainerWrapper" class="hidden">
            <div id="resultContainer" class="fade-in bg-white/80 backdrop-blur-md p-4 sm:p-8 rounded-xl">
                 <header class="text-center mb-8">
                    <h1 class="text-3xl font-bold text-gray-900">DT 역량 진단 결과</h1>
                    <p class="mt-2 text-md text-gray-600"><span id="userDept"></span> 소속 임직원님의 진단 결과입니다.</p>
                </header>
                <div class="result-card">
                    <h2 class="text-xl font-semibold mb-4 text-gray-800">종합 진단</h2>
                    <div class="text-center p-6 bg-blue-50 rounded-lg">
                        <p class="text-lg text-gray-600">귀하의 현재 DT 역량 수준은</p>
                        <p id="overallTier" class="text-3xl font-bold text-blue-600 my-2"></p>
                        <p id="overallDescription" class="text-gray-700 max-w-2xl mx-auto"></p>
                    </div>
                </div>
                <div class="result-card">
                    <h2 class="text-xl font-semibold mb-4 text-gray-800">나의 AI 활용 유형</h2>
                    <div id="aiUseType" class="text-center p-6 bg-green-50 rounded-lg">
                        <p id="aiUseTypeTitle" class="text-2xl font-bold text-green-700 mb-2"></p>
                        <p id="aiUseTypeDescription" class="text-gray-700 max-w-2xl mx-auto"></p>
                    </div>
                </div>
                <div class="grid md:grid-cols-2 gap-6">
                     <div class="result-card">
                        <h2 class="text-xl font-semibold mb-4 text-gray-800">역량 수준 분석</h2>
                        <div> <canvas id="competencyChart"></canvas> </div>
                    </div>
                    <div class="result-card">
                         <h2 class="text-xl font-semibold mb-4 text-gray-800">맞춤 교육 과정 추천</h2>
                         <p id="courseRecommendationText" class="text-gray-600 mb-4"></p>
                         <ul id="recommendedCourses" class="space-y-3"></ul>
                    </div>
                </div>
                <div class="result-card">
                     <h2 class="text-xl font-semibold mb-4 text-gray-800">상세 분석 요약</h2>
                     <p class="text-gray-600 mb-4">귀하는 <strong id="dtNeedAreas" class="text-blue-600"></strong> 영역에서 DT 기술 도입이 시급하다고 응답하셨습니다. 해당 영역의 전문성을 강화하기 위한 맞춤형 학습 플랜을 세워보세요.</p>
                </div>
            </div>
             <div class="text-center mt-8 flex flex-wrap justify-center gap-4">
                <button id="generateReportBtn" class="bg-indigo-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-indigo-700 focus:outline-none focus:ring-4 focus:ring-indigo-300 transition duration-300">
                    ✨ AI 심층 분석 리포트
                </button>
                <button id="downloadPdf" class="bg-green-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-green-700 focus:outline-none focus:ring-4 focus:ring-green-300 transition duration-300">
                    결과지 PDF로 저장
                </button>
                <button id="restartSurvey" class="bg-gray-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-gray-700 focus:outline-none focus:ring-4 focus:ring-gray-300 transition duration-300">
                    다시 진단하기
                </button>
            </div>
        </div>
    </div>

    <!-- Report Modal -->
    <div id="reportModal" class="hidden fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50 p-4">
        <div class="bg-white rounded-lg shadow-2xl max-w-3xl w-full max-h-[90vh] flex flex-col">
            <div class="p-4 border-b flex justify-between items-center">
                <h2 class="text-2xl font-bold text-gray-800">✨ AI 심층 분석 리포트</h2>
                <button id="closeReportModal" class="text-gray-500 hover:text-gray-800 text-3xl">&times;</button>
            </div>
            <div id="reportContent" class="p-6 overflow-y-auto">
                <div id="reportLoader" class="flex justify-center items-center h-64">
                    <div class="loader"></div>
                </div>
                <div id="reportText" class="text-gray-700 leading-relaxed whitespace-pre-wrap"></div>
            </div>
        </div>
    </div>
    
    <!-- Chatbot Floating Button and Modal -->
    <div id="chatbot-container" class="hidden fixed bottom-5 right-5 z-50">
        <button id="chatbot-fab" class="bg-blue-600 text-white w-16 h-16 rounded-full shadow-lg flex items-center justify-center hover:bg-blue-700 transition">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
            </svg>
        </button>
        <div id="chatbot-window" class="hidden fixed bottom-24 right-5 w-96 h-[600px] bg-white rounded-lg shadow-2xl flex flex-col transition-all duration-300 origin-bottom-right">
            <div class="bg-blue-600 text-white p-4 rounded-t-lg flex justify-between items-center">
                <h3 class="font-bold text-lg">AI 역량 컨설턴트</h3>
                <button id="close-chatbot" class="text-white">&times;</button>
            </div>
            <div id="chat-messages" class="flex-1 p-4 overflow-y-auto flex flex-col space-y-4">
                <div class="chat-bubble bot-bubble">
                    안녕하세요! AI 역량 컨설턴트입니다. 진단 결과에 대해 궁금한 점을 해결해 드릴게요. 아래 제안을 선택하거나 직접 질문을 입력해주세요.
                    <div class="mt-2 flex flex-wrap gap-2">
                        <button class="suggestion-chip">내 강점과 약점은?</button>
                        <button class="suggestion-chip">1개월 학습 계획 짜줘</button>
                        <button class="suggestion-chip">AI 리더가 되려면?</button>
                    </div>
                </div>
            </div>
            <div class="p-4 border-t">
                <form id="chat-form" class="flex space-x-2">
                    <input type="text" id="chat-input" class="flex-1 border rounded-full px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="메시지를 입력하세요...">
                    <button type="submit" class="bg-blue-600 text-white rounded-full px-4 py-2 font-semibold hover:bg-blue-700">전송</button>
                </form>
            </div>
        </div>
    </div>


    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // DOM Elements
            const startContainer = document.getElementById('startContainer');
            const startSurveyBtn = document.getElementById('startSurveyBtn');
            const surveyContainer = document.getElementById('surveyContainer');
            const resultContainerWrapper = document.getElementById('resultContainerWrapper');
            const form = document.getElementById('surveyForm');
            const inputs = form.querySelectorAll('input, select');
            const progressBar = document.getElementById('progressBar');
            const restartButton = document.getElementById('restartSurvey');
            const downloadPdfBtn = document.getElementById('downloadPdf');
            const generateReportBtn = document.getElementById('generateReportBtn');
            const reportModal = document.getElementById('reportModal');
            const closeReportModalBtn = document.getElementById('closeReportModal');
            const reportLoader = document.getElementById('reportLoader');
            const reportText = document.getElementById('reportText');
            const chatbotContainer = document.getElementById('chatbot-container');
            const chatbotFab = document.getElementById('chatbot-fab');
            const chatbotWindow = document.getElementById('chatbot-window');
            const closeChatbotBtn = document.getElementById('close-chatbot');
            const chatForm = document.getElementById('chat-form');
            const chatInput = document.getElementById('chat-input');
            const chatMessages = document.getElementById('chat-messages');
            
            // State
            const totalQuestions = 9;
            let competencyChart = null;
            let currentAnalysisResult = null;

            // Event Listeners
            startSurveyBtn.addEventListener('click', () => {
                startContainer.classList.add('hidden');
                surveyContainer.classList.remove('hidden');
                surveyContainer.classList.add('fade-in');
            });
            
            inputs.forEach(input => input.addEventListener('change', updateProgress));
            
            form.addEventListener('submit', function (e) {
                e.preventDefault();
                if (!validateForm()) return;
                
                const formData = new FormData(form);
                const surveyData = {};
                formData.forEach((value, key) => {
                    if (surveyData[key]) {
                        if (!Array.isArray(surveyData[key])) surveyData[key] = [surveyData[key]];
                        surveyData[key].push(value);
                    } else {
                        surveyData[key] = value;
                    }
                });
                displayResults(surveyData);
            });
            
            restartButton.addEventListener('click', () => {
                resultContainerWrapper.classList.add('hidden');
                chatbotContainer.classList.add('hidden');
                surveyContainer.classList.add('hidden');
                startContainer.classList.remove('hidden');
                form.reset();
                updateProgress();
                window.scrollTo(0, 0);
            });
            
            downloadPdfBtn.addEventListener('click', () => {
                 const { jsPDF } = window.jspdf;
                 const resultEl = document.getElementById('resultContainer');
                 const spinner = document.createElement('div');
                 spinner.className = 'loader absolute top-1/2 left-1/2';
                 resultEl.appendChild(spinner);
                 
                 html2canvas(resultEl, { scale: 2, useCORS: true }).then(canvas => {
                    resultEl.removeChild(spinner);
                    const imgData = canvas.toDataURL('image/png');
                    const pdf = new jsPDF({
                        orientation: 'p',
                        unit: 'px',
                        format: [canvas.width, canvas.height]
                    });
                    pdf.addImage(imgData, 'PNG', 0, 0, canvas.width, canvas.height);
                    pdf.save('DT_역량_진단_결과.pdf');
                });
            });

            generateReportBtn.addEventListener('click', generateDetailedReport);
            closeReportModalBtn.addEventListener('click', () => reportModal.classList.add('hidden'));

            chatbotFab.addEventListener('click', () => {
                chatbotWindow.classList.toggle('hidden');
            });

            closeChatbotBtn.addEventListener('click', () => {
                chatbotWindow.classList.add('hidden');
            });
            
            chatForm.addEventListener('submit', async (e) => {
                e.preventDefault();
                const userInput = chatInput.value.trim();
                if (!userInput) return;

                appendMessage(userInput, 'user');
                chatInput.value = '';
                appendMessage(null, 'bot', true); // Show loader

                try {
                    const botResponse = await getChatbotResponse(userInput);
                    const lastMessageDiv = chatMessages.querySelector('.bot-bubble:last-child');
                    const loader = lastMessageDiv.querySelector('.loader');
                    if(loader) loader.remove();
                    lastMessageDiv.textContent = botResponse;
                } catch(error) {
                    console.error("Chatbot API error:", error);
                    const lastMessageDiv = chatMessages.querySelector('.bot-bubble:last-child');
                    lastMessageDiv.textContent = "죄송합니다, 답변을 생성하는 데 문제가 발생했습니다.";
                }
            });
            
            chatMessages.addEventListener('click', function(e) {
                if (e.target.classList.contains('suggestion-chip')) {
                    const suggestionText = e.target.textContent;
                    chatInput.value = suggestionText;
                    chatForm.dispatchEvent(new Event('submit', { bubbles: true, cancelable: true }));
                }
            });

            // Functions
            async function generateDetailedReport() {
                if (!currentAnalysisResult) {
                    alert("먼저 진단을 완료해주세요.");
                    return;
                }
                reportText.innerHTML = '';
                reportLoader.style.display = 'flex';
                reportModal.classList.remove('hidden');
                const systemPrompt = `당신은 HRD 전문 컨설턴트입니다. 사용자의 DT 역량 진단 결과를 바탕으로, 강점과 약점을 심층적으로 분석하고, 성장을 위한 구체적인 액션 플랜을 포함한 상세 리포트를 작성해주세요. 보고서는 격려하는 톤으로 작성하며, 다음 구조를 따라주세요:

1.  **인트로**: 진단 결과에 대한 총평과 함께 사용자("임직원님")를 환영합니다.
2.  **강점 분석**: 가장 높은 점수를 받은 1~2개 역량을 '강점'으로 지정하고, 칭찬과 함께 실제 업무에서 어떻게 더 잘 활용할 수 있을지 구체적인 예시를 들어 설명합니다.
3.  **개선 영역 분석**: 가장 낮은 점수를 받은 1~2개 역량을 '보완이 필요한 영역'으로 지정합니다. 부정적인 표현 대신 '성장 기회'로 표현하며, 이 역량을 왜 키워야 하는지 중요성을 부드럽게 설명합니다.
4.  **✨ 맞춤형 성장 로드맵**: 'AI 탐험가', 'AI 개척자', 'AI 리더' 레벨에 맞는 1개월 단위의 구체적인 학습 계획을 제시합니다. 사용자가 선택한 추천 교육 과정을 반드시 포함하고, 그 외에 온라인 강의, 도서 추천, 사내 스터디 제안 등 실행 가능한 액션 아이템을 2-3가지 제안합니다.
5.  **마무리**: 전체 내용을 요약하고, 꾸준한 성장을 격려하는 긍정적인 메시지로 마무리합니다.

각 섹션 제목은 마크다운 형식의 볼드체로 작성해주세요. (예: **1. 인트로**). 답변은 한국어로 작성해주세요.`;
                const userContext = `
- 종합 진단: ${currentAnalysisResult.tier}
- AI 활용 유형: ${currentAnalysisResult.persona}
- 역량 점수 (4점 만점): ${JSON.stringify(currentAnalysisResult.scores)}
- 희망 교육 과정: ${currentAnalysisResult.courses}

위 사용자의 진단 결과에 대한 심층 분석 리포트를 작성해주세요.`;
                try {
                    const apiKey = "";
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
                    const payload = {
                        contents: [{ parts: [{ text: userContext }] }],
                        systemInstruction: { parts: [{ text: systemPrompt }] },
                    };
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                    const result = await response.json();
                    const report = result.candidates?.[0]?.content?.parts?.[0]?.text || "리포트를 생성하는 데 실패했습니다.";
                    const formattedReport = report.replace(/\*\*(.*?)\*\*/g, '<strong class="font-bold text-gray-900">$1</strong>');
                    reportText.innerHTML = formattedReport;
                } catch (error) {
                    console.error("Report generation error:", error);
                    reportText.textContent = "죄송합니다. 리포트를 생성하는 중 오류가 발생했습니다. 잠시 후 다시 시도해주세요.";
                } finally {
                    reportLoader.style.display = 'none';
                }
            }

            function updateProgress() {
                const answeredCheckboxGroups = new Set();
                form.querySelectorAll('input[type="checkbox"]').forEach(cb => { if (cb.checked) answeredCheckboxGroups.add(cb.name); });
                const answeredRadioAndSelect = new Set([...form.querySelectorAll('input[type="radio"]:checked, select:valid')].map(input => input.name));
                const answeredCount = answeredRadioAndSelect.size + answeredCheckboxGroups.size;
                progressBar.style.width = `${(answeredCount / totalQuestions) * 100}%`;
            }
            
            function validateForm() {
                const isQ8Valid = validateCheckboxes('q8_container');
                const isQ9Valid = validateCheckboxes('q9_container');
                if (!form.checkValidity() || !isQ8Valid || !isQ9Valid) {
                    const firstInvalid = form.querySelector(':invalid');
                    if (firstInvalid) {
                        firstInvalid.focus();
                        (firstInvalid.closest('fieldset') || firstInvalid.closest('div')).scrollIntoView({ behavior: 'smooth', block: 'center' });
                    }
                    return false;
                }
                return true;
            }

            function validateCheckboxes(containerId) {
                const container = document.getElementById(containerId);
                const isChecked = Array.from(container.querySelectorAll('input[type="checkbox"]')).some(cb => cb.checked);
                container.classList.toggle('border', !isChecked);
                container.classList.toggle('border-red-500', !isChecked);
                container.classList.toggle('p-4', !isChecked);
                container.classList.toggle('rounded-md', !isChecked);
                if (!isChecked) container.scrollIntoView({ behavior: 'smooth', block: 'center' });
                return isChecked;
            }
            
            async function getChatbotResponse(userQuery) {
                if (!currentAnalysisResult) return "진단 결과가 없습니다. 먼저 진단을 완료해주세요.";

                const systemPrompt = `당신은 사용자의 DT(디지털 전환) 역량 진단 결과를 분석하고 맞춤형 조언을 제공하는 전문 'AI 역량 컨설턴트'입니다. 다음은 사용자의 진단 결과입니다.
                - 종합 진단: ${currentAnalysisResult.tier}
                - AI 활용 유형: ${currentAnalysisResult.persona}
                - 역량 점수 (4점 만점): ${JSON.stringify(currentAnalysisResult.scores)}
                - 추천 교육 과정: ${currentAnalysisResult.courses}
                
                이 정보를 바탕으로, 사용자의 질문에 대해 구체적이고 실행 가능한 해결책을 친절한 전문가 톤으로 제시해주세요. 답변은 한국어로 해주세요.`;

                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                const payload = {
                    contents: [{ parts: [{ text: userQuery }] }],
                    systemInstruction: { parts: [{ text: systemPrompt }] },
                };

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const result = await response.json();
                return result.candidates?.[0]?.content?.parts?.[0]?.text || "답변을 가져올 수 없습니다.";
            }
            
            function appendMessage(text, sender, isLoading = false) {
                const messageDiv = document.createElement('div');
                messageDiv.className = `chat-bubble ${sender}-bubble`;
                if(isLoading) {
                    const loader = document.createElement('div');
                    loader.className = 'loader';
                    messageDiv.appendChild(loader);
                } else {
                    messageDiv.textContent = text;
                }
                chatMessages.appendChild(messageDiv);
                chatMessages.scrollTop = chatMessages.scrollHeight;
            }

            function displayResults(data) {
                surveyContainer.classList.add('hidden');
                resultContainerWrapper.classList.remove('hidden');
                chatbotContainer.classList.remove('hidden');
                window.scrollTo(0, 0);

                const scores = {
                    'DT 인식': (parseInt(data.dt_status, 10) + parseInt(data.process_digital, 10)) / 2,
                    'DT 적극성': parseInt(data.dt_proactive, 10) / 5 * 4,
                    'AI 이해': parseInt(data.ai_understanding, 10),
                    '데이터 분석': parseInt(data.data_experience, 10),
                    '프로그래밍': parseInt(data.programming_experience, 10)
                };
                
                const averageScore = Object.values(scores).reduce((a, b) => a + b, 0) / Object.keys(scores).length;

                document.getElementById('userDept').textContent = data.department || "참여자";
                const overallTier = document.getElementById('overallTier');
                const overallDescription = document.getElementById('overallDescription');
                let recommendationText = '';

                if (averageScore >= 3.2) {
                    overallTier.textContent = "🚀 AI 리더 (AI Leader)";
                    overallDescription.textContent = "AI 기술에 대한 깊은 이해를 바탕으로 비즈니스 전략과 연계하여 새로운 가치를 창출하는 단계입니다. 데이터 기반의 의사결정을 주도하고, 조직의 DT 전략을 수립하는 데 기여할 수 있는 핵심 인재입니다. 최신 기술 트렌드를 지속적으로 학습하고, 조직 전체의 DT 역량을 이끌어가는 역할을 수행해보세요.";
                    recommendationText = "현재 역량 수준을 더욱 고도화하기 위해 다음의 심화 과정을 추천합니다.";
                } else if (averageScore >= 2.2) {
                    overallTier.textContent = "🏃 AI 개척자 (AI Pioneer)";
                    overallDescription.textContent = "다양한 AI 툴과 디지털 기술을 실제 업무에 적용하며 생산성을 향상시키는 단계입니다. 특정 업무 영역(예: 데이터 분석, 업무 자동화)에 대한 깊이 있는 학습을 통해 자신만의 전문 분야를 만들어가는 것을 추천합니다. 작은 성공 경험을 쌓아 조직 내 AI 활용을 확산시키는 역할을 할 수 있습니다.";
                    recommendationText = "선택하신 희망 과정들을 통해 전문성을 한 단계 높여보세요.";
                } else {
                    overallTier.textContent = "🧐 AI 탐험가 (AI Explorer)";
                    overallDescription.textContent = "AI와 디지털 기술의 가능성을 탐색하고 기본 개념을 학습하는 단계입니다. AI 리터러시, 데이터 리터러시와 같은 기초 역량을 쌓아 비즈니스 문제 해결의 첫 걸음을 내딛는 것이 중요합니다. 다양한 성공 사례를 접하며 AI가 어떻게 업무를 변화시킬 수 있는지 큰 그림을 그려보세요.";
                    recommendationText = "기초 역량을 탄탄히 다지기 위해 다음의 기본 과정을 추천합니다.";
                }
                
                const useType = determineAiUseType(data);
                document.getElementById('aiUseTypeTitle').textContent = useType.title;
                document.getElementById('aiUseTypeDescription').textContent = useType.description;

                const chartCanvas = document.getElementById('competencyChart');
                const chartData = {
                    labels: Object.keys(scores),
                    datasets: [{
                        label: '나의 역량 점수', data: Object.values(scores), fill: true,
                        backgroundColor: 'rgba(59, 130, 246, 0.2)', borderColor: 'rgb(59, 130, 246)',
                        pointBackgroundColor: 'rgb(59, 130, 246)', pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff', pointHoverBorderColor: 'rgb(59, 130, 246)'
                    }]
                };
                if (competencyChart) competencyChart.destroy();
                competencyChart = new Chart(chartCanvas, {
                    type: 'radar', data: chartData,
                    options: { elements: { line: { borderWidth: 3 } }, scales: { r: { angleLines: { display: true }, suggestedMin: 0, suggestedMax: 4, pointLabels: { font: { size: 14 } } } }, plugins: { legend: { display: false } } }
                });

                document.getElementById('courseRecommendationText').textContent = recommendationText;
                const coursesList = document.getElementById('recommendedCourses');
                coursesList.innerHTML = '';
                const recommended = Array.isArray(data.training_course) ? data.training_course : [data.training_course];
                
                if (recommended.includes("희망 과정 없음") || recommended.length === 0) {
                     coursesList.innerHTML = `<li class="flex items-start"><span class="mr-3 text-gray-500">✓</span><span class="text-gray-700">희망하는 교육 과정이 없는 것으로 확인되었습니다.</span></li>`;
                } else {
                    recommended.forEach(course => {
                        const li = document.createElement('li');
                        li.className = 'flex items-start p-3 bg-gray-50 rounded-md';
                        li.innerHTML = `<span class="mr-3 text-green-500 font-bold">✓</span><span class="text-gray-800">${course}</span>`;
                        coursesList.appendChild(li);
                    });
                }

                const needs = Array.isArray(data.dt_need_area) ? data.dt_need_area.join(', ') : data.dt_need_area;
                document.getElementById('dtNeedAreas').textContent = needs;
                
                currentAnalysisResult = {
                    tier: overallTier.textContent,
                    persona: useType.title,
                    scores: scores,
                    courses: recommended.join(', ')
                };
            }
            
            function determineAiUseType(data) {
                const needs = Array.isArray(data.dt_need_area) ? data.dt_need_area : [data.dt_need_area];
                const courses = Array.isArray(data.training_course) ? data.training_course : [data.training_course];
                if (needs.includes('데이터 분석/시각화') || courses.includes('[심화] Python 데이터 분석')) return { title: '📊 데이터 기반 의사결정가', description: '데이터를 분석하고 시각화하여 객관적인 근거에 기반한 의사결정을 내리는 데 강점이 있습니다. Python, BI 툴 등을 활용한 심화 학습으로 데이터 전문가로 성장할 수 있습니다.' };
                if (needs.includes('행정업무 자동화') || courses.includes('[심화] 업무 자동화 실습')) return { title: '⚙️ 프로세스 효율화 전문가', description: '반복적인 업무를 자동화하여 조직 전체의 생산성을 높이는 데 기여할 수 있습니다. RPA, 노코드 툴 등을 적극적으로 활용하여 업무 프로세스를 혁신해보세요.' };
                if (needs.includes('고객 관리/마케팅')) return { title: '💡 비즈니스 혁신가', description: 'AI 기술을 고객 관리, 마케팅 등 비즈니스 핵심 영역에 적용하여 새로운 성장 기회를 만드는 데 관심이 많습니다. AI 활용 사례 학습을 통해 창의적인 아이디어를 발굴해보세요.' };
                return { title: '🌟 잠재적 멀티플레이어', description: '다양한 영역에서 DT/AI 기술의 활용 가능성을 탐색하고 있습니다. 가장 관심 있는 분야를 정해 기본기부터 차근차근 학습하며 자신만의 전문성을 키워나가는 것을 추천합니다.' };
            }

            updateProgress();
        });
    </script>
</body>
</html>

