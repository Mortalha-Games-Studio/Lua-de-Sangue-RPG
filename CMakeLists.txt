cmake_minimum_required(VERSION 3.16)
project(MeuJogo2D VERSION 0.1.0 LANGUAGES CXX)

# Define o padrão do C++ (C++17 ou C++20 são ótimos para jogos)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Baixa e compila a biblioteca gráfica (Raylib) automaticamente
include(FetchContent)
FetchContent_Declare(
    raylib
    GIT_REPOSITORY https://github.com
    GIT_TAG 5.5 # Versão estável
    GIT_SHALLOW TRUE
)
FetchContent_MakeAvailable(raylib)

# Busca automaticamente todos os arquivos .cpp dentro da pasta src/
file(GLOB_RECURSE SOURCES "src/*.cpp")

# Cria o executável do jogo
add_executable(${PROJECT_NAME} ${SOURCES})

# Inclui a pasta de cabeçalhos (se houver)
target_include_directories(${PROJECT_NAME} PRIVATE src)

# Vincula a biblioteca ao seu jogo
target_link_libraries(${PROJECT_NAME} PRIVATE raylib)

# Copia a pasta 'assets' para o diretório do executável após compilar
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/assets $<TARGET_FILE_DIR:${PROJECT_NAME}>/assets
)
