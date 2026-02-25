# Інструкція для встановлення R
Інструкція для встановлення R: https://www.jichi.ac.jp/usr/hema/EZR/statmedEN.html
Для Mac: https://www.jichi.ac.jp/usr/hema/EZR/OSXEN.html
1. Завантаження та встановлення R: https://cloud.r-project.org
    Для Mac на М-чіпах: R-4.5.2-arm64.pkg, пряме посилання: https://cloud.r-project.org/bin/macosx/big-sur-arm64/base/R-4.5.2-arm64.pkg
2. Завантаження та встановлення XQuartz: https://www.xquartz.org
3. Перезавантажити комʼпютер
4. Запуск R та в консолі ввести команду: install.packages("RcmdrPlugin.EZR", dependencies=TRUE) та вибрати дзеркало ближче до України, наприклад Poland, можливо після цієї команди потрібно ще раз перезавантажити компʼютер, якщо наступна команда буде видавати помилки. Також при наявності помилки ввести в консолі: install.packages("Rcmdr", dependencies=TRUE) та знову перезавантажити компʼютер якщо наступна команда не буде працювати.
5. Можливо потрібно буде налаштувати шляхи Tcl/Tk, що йдуть з R.app ARM64:
    Визначаємо шляхи:
        file.path(R.home(), "lib", "tcl8.6")
        file.path(R.home(), "lib", "tk8.6")
    Встановлюємо змінні середовища Tcl/Tk для поточної сесії R з правильними шляхами:
        Sys.setenv(TCL_LIBRARY="/Library/Frameworks/R.framework/Resources/lib/tcl8.6")
        Sys.setenv(TK_LIBRARY="/Library/Frameworks/R.framework/Resources/lib/tk8.6")
5. Запуск R commander(EZR) - в консолі R ввести: library(Rcmdr)

# Робота у Visual Studio Code
Для роботи з кодом мови програмування R у Visual Studio Code(VS Code):
1. Має бути встановлений R із https://cloud.r-project.org
2. В консолі R встановити R-пакет: install.packages("languageserver")
3. Завантажити розширення для VS Code: R Extension for Visual Studio Code від REditorSupport: https://marketplace.visualstudio.com/items?itemName=reditorsupport.r
4. Створити файл із розширенням .R, наприклад test.R. Написати потрібний код.
5. Для запуску файлу:
    1. Нажати кнопку ▶ Run Source (Cmd + Shift + S) (результати можуть не виводитись і для цього потрібно обовґязково вказувати print(data))
    2. Або виділити фрагмент коду та нажати Cmd + Enter
    3. Або через термінал VS Code, в якому ввести: R + Enter, далі в середовищі R можна ввести весь код вручну або ввести команду запуску файлу(з цією командою результат буде виводитись): source("test.R"). Робочу директорію можна дізнатись командою: getwd(), або використовувати абсолютні шляхи: source("/Users/Admin/Desktop/test.R")
6. Якщо графічні елементи R не будуть відображатись то перезапустити VS Code

# Автоматичне встановлення шляхів до Tcl/Tk для Mac
Sys.setenv(TCL_LIBRARY="/Library/Frameworks/R.framework/Resources/lib/tcl8.6")
Sys.setenv(TK_LIBRARY="/Library/Frameworks/R.framework/Resources/lib/tk8.6")

# Функція для перезапуску Rcmdr на Mac (коли вікно R Commander закрито через графічний інтерфейс, то щоб не перезаватажувати R то запускати нище наведену функцію в консолі командою(попередньо прописати саму функцію в консолі): restartRcmdr())
restartRcmdr <- function() {
  if ("package:Rcmdr" %in% search()) detach("package:Rcmdr", unload=TRUE)
  if ("package:tcltk" %in% search()) detach("package:tcltk", unload=TRUE)
  
  Sys.setenv(TCL_LIBRARY="/Library/Frameworks/R.framework/Resources/lib/tcl8.6")
  Sys.setenv(TK_LIBRARY="/Library/Frameworks/R.framework/Resources/lib/tk8.6")
  
  library(Rcmdr)
}

# Функція автоматичного запуску R Commander при старті R:
options(Rcmdr=list(plugins='RcmdrPlugin.EZR'))
local({
    old <- getOption('defaultPackages')
    options(defaultPackages = c(old, 'Rcmdr'))
})

addTaskCallback(function(expr, value, ok, visible) {
  if (capabilities("tcltk") && interactive()) {
    library(Rcmdr)
    return(FALSE)
  }
  TRUE
})

Для автоматизації можна в домашній директорії(/Users/Admin/) створити файл з назвою: .Rprofile і перенести дані функції в цей файл.


# Shiny пакет для інтеграції з Web
1. Встановлення - в командному рядку R ввести: install.packages("shiny")
2. Перевірка тестового прикладу:
    library(shiny)
    runExample("01_hello")
3. Має відкритись сайт за локальною адресою та портом, наприклад http://127.0.0.1:xxxx (буде написано в консолі)
4. Для зупинки серверу нажати в консолі Esc
5. Для запуску конкретного файлу писати в консолі команду: runApp("шлях/до/app.R"): runApp("/Users/Admin/Аспірантура/Предмети/Біостатистика/Статистичні пакети/R/app.R")

Додатково для роботи веб-додатку можна встановити: install.packages("DT", dependencies = TRUE)

# Додатково для стильних графіків:
1. install.packages("plotly"). Інтерактивні графіки (зум, hover, панорамування). Підтримує як базові plot(), так і ggplot2. Можна робити лінії, діаграми, 3D-графіки, карти.
2. install.packages("highcharter") - Побудований на JS-бібліотеці Highcharts. “Дизайнерські” графіки, анімації, кастомні tooltip’и. Підходить для стовпчикових, лінійних, кругових, карт та ін.
    Може бути потрібні ці пакети install.packages(c("htmlwidgets", "magrittr", "RColorBrewer", "scales"))
3. install.packages("echarts4r") - Інтерактивні графіки на базі Apache ECharts. Ці synthfrnbdys графіки гарно виглядають за замовчуванням, можна робити анімації, інтегрувати в Shiny
4. install.packages("ggiraph") - цей пакет може додавати інтерактивність для ggplot2

Ще пакети, що дають інтерактивність:
htmlwidgets - базис для інтерактивних HTML
DT - інтерактивні таблиці
reactable - сучасні інтерактивні таблиці
leaflet - інтерактивні карти
visNetwork - інтерактивні графи
networkD3 - D3‑графи
DTedit - редаговані таблиці
formattable - красиві табличні формати
collapsibleTree - інтерактивні деревовидні діаграми
plotlygeo - інтерактивні карти
timevis - інтерактивні часові лінії

# R Markdown
install.packages("rmarkdown")
Sys.setenv(RSTUDIO_PANDOC="/opt/homebrew/bin")
В R Console запускати для html, docx, ioslides, beamer, pptx: rmarkdown::render("/Users/Admin/Аспірантура/Предмети/Біостатистика/Статистичні пакети/R/report.Rmd")

Варіанти виходу файлів:
output: html_document

output: ioslides_presentation

output: beamer_presentation

output: word_document
always_allow_html: true

output: powerpoint_presentation

output:
  pdf_document:
    latex_engine: xelatex
В R Console запускати для PDF(потрібен пакет: install.packages("tinytex") потім: tinytex::install_tinytex()): rmarkdown::render("/Users/Admin/Аспірантура/Предмети/Біостатистика/Статистичні пакети/R/report.Rmd", output_format = "pdf_document")
