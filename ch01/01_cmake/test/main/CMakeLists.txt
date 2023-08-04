# CMake ��Ͱ汾��Ҫ��
cmake_minimum_required(VERSION 2.8)

# ��Ŀ��Ϣ
project (Demo4)

# ����һ�������ļ������ڴ��� CMake ��Դ�������
configure_file (
    "${PROJECT_SOURCE_DIR}/config.h.in"
	"${PROJECT_BINARY_DIR}/config.h"
)

# �Ƿ�ʹ���Լ��� MathFunctions ��
option(USE_MYMATH "Use provided math implementation" ON)

# �Ƿ���� MathFunctions ��
if (USE_MYMATH)
    include_directories("${PROJECT_SOURCE_DIR}/math")
	add_subdirectory(math)
	set(EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif(USE_MYMATH)

# ���ҵ�ǰĿ¼�µ�����Դ�ļ�
# �������Ʊ��浽 DIR_SRCS ����
aux_source_directory(. DIR_SRCS)

# ָ������Ŀ¼
add_executable(Demo ${DIR_SRCS})

# �������ӿ�
target_link_libraries(Demo ${EXTRA_LIBS})

# ָ����װ·��
install (TARGETS Demo DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/config.h" DESTINATION include)

# ���ò���
enable_testing()

# ���Գ����Ƿ�ɹ�����
add_test(test_run Demo 5 2)

# ���԰�����Ϣ�Ƿ����������ʾ
add_test(test_usage Demo)
set_tests_properties (test_usage PROPERTIES PASS_REGULAR_EXPRESSION "Usage: .* base exponent")

# ���� 5 ��ƽ��
add_test(test_5_2 Demo 5 2)
set_tests_properties (test_5_2 PROPERTIES PASS_REGULAR_EXPRESSION "is 25")

# ���� 10 �� 5 �η�
add_test (test_10_5 Demo 10 5)
set_tests_properties (test_10_5 PROPERTIES PASS_REGULAR_EXPRESSION "is 100000")

# ���� 2 �� 10 �η�
add_test (test_2_10 Demo 2 10)
set_tests_properties (test_2_10 PROPERTIES PASS_REGULAR_EXPRESSION "is 1024")

# ����һ���꣬�����򻯲��Թ���
macro (do_test arg1 arg2 result)
  add_test(test_${arg1}_${arg2} Demo ${arg1} ${arg2})
  set_tests_properties(test_${arg1}_${arg2} PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endmacro(do_test)

# ʹ�øú����һϵ�е����ݲ���
do_test(6 2 "is 36")
do_test(10 4 "is 10000")
do_test(2 11 "is 2048")