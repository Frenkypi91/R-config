# ============================ SESSION DETECTION ============================
.is_rstudio <- function() {
  Sys.getenv("RSTUDIO") == "1" || nzchar(Sys.getenv("RSTUDIO_SESSION_PORT"))
}

.supports_ansi <- function() {
  term <- Sys.getenv("TERM", "dumb")
  ok_term <- term != "dumb"
  ok_tty  <- interactive() && (isatty(stdout()) || isatty(stderr()))
  ok_term && ok_tty && !.is_rstudio()
}

# ============================== ANSI COLORS ===============================
.ansi <- list(
  reset   = "\033[0m",
  bold    = "\033[1m",
  dim     = "\033[2m",
  red     = "\033[31m",
  green   = "\033[32m",
  yellow  = "\033[33m",
  blue    = "\033[34m",
  magenta = "\033[35m",
  cyan    = "\033[36m",
  gray    = "\033[90m"
)

.style <- function(text, ...) {
  if (!.supports_ansi()) return(text)
  codes <- paste0(unlist(list(...)), collapse = "")
  paste0(codes, text, .ansi$reset)
}

# =============================== LOG HELPERS ==============================
cat_ok    <- function(...) cat(.style(paste0(...), .ansi$green), "\n", sep = "")
cat_info  <- function(...) cat(.style(paste0(...), .ansi$cyan), "\n", sep = "")
cat_warn  <- function(...) cat(.style(paste0(...), .ansi$yellow), "\n", sep = "")
cat_error <- function(...) cat(.style(paste0(...), .ansi$red, .ansi$bold), "\n", sep = "")
cat_dim   <- function(...) cat(.style(paste0(...), .ansi$gray), "\n", sep = "")

# ============================== HISTORY ===================================
if (interactive() && file.exists(path.expand("~/.Rhistory"))) {
  try(utils::loadhistory(path.expand("~/.Rhistory")), silent = TRUE)
}

.Last <- function() {
  if (interactive()) {
    try(utils::savehistory(path.expand("~/.Rhistory")), silent = TRUE)
  }
}

# =============================== OVERRIDE =================================
q <- function(save = "no", status = 0, runLast = TRUE) {
  base::q(save = save, status = status, runLast = runLast)
}

# ================================= REPOS ==================================
options(repos = c(CRAN = "https://cloud.r-project.org"))

# =============================== USER LIB =================================
userlib <- path.expand("~/.r")
dir.create(userlib, recursive = TRUE, showWarnings = FALSE)
.libPaths(c(userlib, .libPaths()))

# ============================ DEFAULT OPTIONS =============================
options(
  stringsAsFactors = FALSE,
  scipen = 999,
  digits = 4,
  width  = 100
)

# =========================== SHELL-LIKE ALIASES ===========================
cls <- function() rm(list = ls(envir = .GlobalEnv), envir = .GlobalEnv)
clc <- function() cat("\014")
pwd <- function() getwd()
cd  <- function(x) setwd(x)

ll <- function(path = ".") {
  fs <- file.info(list.files(path, all.files = FALSE))
  fs$size <- format(fs$size, big.mark = ",")
  print(fs[, c("size", "mtime")])
}

# ============================ PATH SHORTENER ==============================
.short_path <- function(n = 2) {
  wd   <- normalizePath(getwd(), winslash = "/", mustWork = FALSE)
  home <- normalizePath(Sys.getenv("HOME"), winslash = "/", mustWork = FALSE)

  # usa ~ solo per HOME
  if (startsWith(wd, home)) {
    wd <- sub(paste0("^", home), "~", wd)
  }

  parts <- strsplit(wd, "/", fixed = TRUE)[[1]]

  # mostra tutto finché i livelli sono <= n
  if (length(parts) <= n + 1) return(wd)

  # appena diventano > n → solo ultime n directory
  paste(tail(parts, n), collapse = "/")
}

# ================================ PROMPT ==================================
.local_prompt <- function() {
  path <- .short_path(2)

  if (.supports_ansi()) {
    paste0(
      .ansi$reset,
      path,
      .ansi$cyan, " » ",
      .ansi$reset
    )
  } else {
    paste0(path, " > ")
  }
}

.update_prompt <- function() {
  options(prompt = .local_prompt())
}

if (interactive()) {
  invisible(.update_prompt())
  invisible(addTaskCallback(
    function(...) { .update_prompt(); TRUE },
    name = "short_path_prompt"
  ))
}
