# CV Export Tool

Ferramenta para exportar o CV JSON para formatos visuais (PDF e DOCX) com template otimizado para ATS.

## Instalação

```bash
cd export
pip install -r requirements.txt
```

### Engines PDF Suportadas

O script suporta duas engines de geração de PDF:

1. **xhtml2pdf** (Recomendado) - Pure Python, funciona em qualquer plataforma
   ```bash
   pip install xhtml2pdf
   ```

2. **WeasyPrint** (Alternativa) - Requer dependências do sistema (GTK3)

   **Windows:**
   ```bash
   # Instalar GTK3 via MSYS2 ou baixar binários
   # https://weasyprint.readthedocs.io/en/stable/first_steps.html#windows
   ```

   **macOS:**
   ```bash
   brew install pango
   ```

   **Linux (Ubuntu/Debian):**
   ```bash
   sudo apt-get install libpango-1.0-0 libpangocairo-1.0-0
   ```

O script tentará usar `xhtml2pdf` primeiro. Se não estiver disponível, tentará `weasyprint`.

## Uso

### Exportar todas as versões (recomendado)
```bash
python export_cv.py --all-langs
```

### Exportar versão específica
```bash
# Português, todos os formatos
python export_cv.py --lang pt-BR

# Inglês, todos os formatos
python export_cv.py --lang en-US

# Apenas PDF
python export_cv.py --format pdf

# Apenas DOCX
python export_cv.py --format docx
```

### Opções completas
```bash
python export_cv.py --help
```

## Template

### Template ATS (`ats_template.html`)
Otimizado para sistemas de rastreamento de candidatos (ATS):
- Layout single-column
- Fontes padrão (Calibri, Arial)
- Headers padronizados que ATS reconhecem
- Sem gráficos ou elementos visuais complexos
- Keywords em texto (não em barras de progresso)
- Alta taxa de parsing por sistemas como Workday, Greenhouse, Lever, Taleo

**Uso:** Candidaturas online, sistemas de RH automatizados, aplicações em plataformas corporativas

### Formato DOCX
Formato Word com estrutura ATS-friendly:
- Compatível com Microsoft Word e LibreOffice
- Editável pelo usuário
- Formatação profissional
- Otimizado para ATS

## Arquivos Gerados

O padrão de nomenclatura dos arquivos segue o formato: `CV - Ed Costa (YYYY-MM-DD) - {idioma}.{extensão}`

Se o arquivo já existir, será adicionado `(n)` antes da extensão: `CV - Ed Costa (YYYY-MM-DD) - {idioma} (1).{extensão}`

```
output/
├── CV - Ed Costa (2026-01-22) - pt-BR.pdf       # PDF ATS em português
├── CV - Ed Costa (2026-01-22) - pt-BR.docx      # DOCX em português
├── CV - Ed Costa (2026-01-22) - en-US.pdf       # PDF ATS em inglês
└── CV - Ed Costa (2026-01-22) - en-US.docx      # DOCX em inglês
```

### Controle de Versão Automático

Ao executar a exportação múltiplas vezes no mesmo dia, o sistema automaticamente incrementa:
```
CV - Ed Costa (2026-01-22) - pt-BR.pdf
CV - Ed Costa (2026-01-22) - pt-BR (1).pdf
CV - Ed Costa (2026-01-22) - pt-BR (2).pdf
...
```

## Customização

### Adicionar novo template
1. Criar `templates/meu_template.html`
2. Usar variáveis Jinja2 disponíveis:
   - `basics` - dados básicos (nome, email, etc.)
   - `work` - experiências profissionais
   - `education` - formação acadêmica
   - `skills` - competências
   - `languages` - idiomas
   - `certificates` - certificações
   - `awards` - premiações
   - `publications` - publicações
   - `labels` - rótulos traduzidos
   - `lang` - idioma atual

### Filtros Jinja2 disponíveis
- `format_date` - formata datas (ex: "2024-03-01" → "Mar/2024")
- `language_percent` - converte nível de idioma para percentual

## Internacionalização (i18n)

O sistema usa as traduções definidas em `x-i18n` no arquivo JSON do CV.
Labels de seções são traduzidos automaticamente baseado no idioma selecionado.

## Troubleshooting

### PDF engine não encontrado
```bash
# Instalar xhtml2pdf (recomendado)
pip install xhtml2pdf

# OU instalar weasyprint (requer GTK3)
pip install weasyprint
```

Se WeasyPrint continuar com erro, verificar dependências do sistema (GTK3, Pango).

### python-docx não encontrado
```bash
pip install python-docx
```

### Fontes não renderizando corretamente no PDF
Instalar fontes Calibri ou Arial no sistema. Em Linux:
```bash
sudo apt-get install ttf-mscorefonts-installer
```
