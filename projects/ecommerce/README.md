# E-Commerce Database Application

> Complete database application with interactive dashboard for e-commerce sales intelligence

## 🚀 Quick Start

### For Google Colab (Easiest)

1. Upload `ecommerce_dashboard.ipynb`
2. Runtime → Run all
3. Done! (20 seconds)

### For Local Jupyter

```bash
pip install pandas numpy faker plotly ipywidgets jupyter
jupyter nbextension enable --py widgetsnbextension
jupyter notebook ecommerce_dashboard.ipynb
# Then: Cell → Run All
```

## 📁 Files

- **`ecommerce_dashboard.ipynb`** ⭐ Main file - Submit this!
- **`QUICKSTART.md`** - Quick start guide
- **`TESTING.md`** - Testing & troubleshooting
- **`FINAL_SUMMARY.md`** - Complete summary
- `analysis/` - Database component (modular version)
- `dashboard/` - Dashboard component (modular version)

## ✅ Features

- **Database**: 4 tables, 16,500+ records
- **SQL**: Aggregations, joins, window functions, CTEs
- **Dashboard**: 3 filters, 4 KPIs, 7 visualizations
- **Interactive**: Real-time filtering with ipywidgets

## 🎯 For Submission

**Submit**: `ecommerce_dashboard.ipynb`

**Why**: Single file, no dependencies, works everywhere!

## 📚 Documentation

1. **QUICKSTART.md** - How to run (start here!)
2. **TESTING.md** - Validation & troubleshooting
3. **FINAL_SUMMARY.md** - Complete project summary
4. **README_COMBINED.md** - Detailed combined docs
5. **PROJECT_SUMMARY.md** - Project overview

## 🆘 Common Errors

### NameError: name 'widgets' is not defined

**Fix**: Run Cell → Run All (don't skip cells!)

### ModuleNotFoundError

```bash
pip install ipywidgets plotly faker pandas numpy
jupyter nbextension enable --py widgetsnbextension
```

### Widgets not interactive

```bash
jupyter nbextension enable --py widgetsnbextension
```

## 📊 Assignment Requirements

| Requirement | Status |
|-------------|--------|
| Multi-table database | ✅ 4 tables |
| Batch data | ✅ 16,500+ records |
| Aggregations | ✅ SUM, AVG, COUNT |
| Joins | ✅ INNER, LEFT |
| Window Functions | ✅ RANK(), AVG() OVER() |
| Subqueries | ✅ CTEs |
| Interactive dashboard | ✅ 3 filters, 7 charts |

**Result**: ✅ ALL REQUIREMENTS MET

## 👨‍🎓 Author

**Baratov Sokhibjon** (12225259)  
Database Management - Inha University - Fall 2025

---

**Status**: ✅ Complete and ready for submission!
