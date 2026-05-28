<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EWE Store Tunja - CRM Profesional</title>
    <!-- Iconos Phosphor -->
    <script src="https://unpkg.com/@phosphor-icons/web"></script>
    <!-- Librería de Gráficas (Chart.js) -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <style>
        /* --- VARIABLES Y RESET --- */
        :root {
            --primary: #072870;
            --primary-hover: #0a3590;
            --accent: #0984e3;        
            --bg: #f5f7fa;            
            --surface: #ffffff;       
            --text: #2d3436;          
            --text-light: #636e72;    
            --border: #dfe6e9;        
            --danger: #d63031;        
            --warning: #fdcb6e;       
            --success: #00b894;       
            --shadow: 0 4px 6px rgba(0,0,0,0.05);
            --radius: 12px;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; }

        body { background-color: var(--bg); color: var(--text); display: flex; height: 100vh; overflow: hidden; }

        /* --- SIDEBAR --- */
        aside {
            width: 260px;
            background: var(--surface);
            border-right: 1px solid var(--border);
            display: flex;
            flex-direction: column;
            padding: 1.5rem;
            z-index: 10;
        }

        .brand {
            font-size: 1.2rem;
            font-weight: 800;
            color: var(--primary);
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            text-transform: uppercase; 
            letter-spacing: 0.5px;
        }

        nav button {
            background: none;
            border: none;
            width: 100%;
            padding: 0.8rem 1rem;
            text-align: left;
            cursor: pointer;
            color: var(--text-light);
            border-radius: var(--radius);
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 0.8rem;
            transition: all 0.2s;
            margin-bottom: 0.5rem;
            font-size: 0.9rem;
        }

        nav button:hover, nav button.active { background-color: #eef2f5; color: var(--primary-hover); }
        nav button i { font-size: 1.2rem; }

        /* --- MAIN CONTENT --- */
        main { flex: 1; padding: 2rem; overflow-y: auto; position: relative; }

        header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem; }
        
        h1 { font-size: 1.8rem; color: var(--primary); }

        .btn {
            padding: 0.6rem 1.2rem;
            border-radius: var(--radius);
            border: none;
            cursor: pointer;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            transition: 0.2s;
            font-size: 0.9rem;
        }

        .btn-primary { background: var(--primary); color: white; }
        .btn-primary:hover { background: var(--primary-hover); }
        .btn-outline { background: transparent; border: 1px solid var(--border); color: var(--text); }
        .btn-outline:hover { border-color: var(--primary); color: var(--primary); }
        .btn-sm { padding: 0.4rem 0.8rem; font-size: 0.8rem; }
        .btn-danger { background: var(--danger); color: white; }
        .btn-icon { padding: 0.5rem; border-radius: 6px; background: #f1f2f6; border: none; cursor: pointer; color: var(--text); }
        .btn-icon:hover { background: #e2e6ea; color: var(--primary); }

        /* --- GRIDS --- */
        .stats-grid, .kpi-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .stat-card, .kpi-card {
            background: var(--surface);
            padding: 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            border: 1px solid var(--border);
        }

        .stat-card h3, .kpi-card h3 { font-size: 0.85rem; color: var(--text-light); margin-bottom: 0.5rem; text-transform: uppercase; }
        .stat-card .value, .kpi-card .value { font-size: 1.8rem; font-weight: 700; color: var(--primary); }

        /* --- CHARTS --- */
        .charts-grid { display: grid; grid-template-columns: 2fr 1fr; gap: 1.5rem; margin-bottom: 2rem; }
        .chart-card { background: var(--surface); padding: 1.5rem; border-radius: var(--radius); box-shadow: var(--shadow); border: 1px solid var(--border); height: 350px; position: relative; }

        /* --- KANBAN --- */
        .kanban-board { display: flex; gap: 1rem; overflow-x: auto; height: calc(100vh - 280px); padding-bottom: 1rem; }
        .kanban-column { min-width: 300px; background: #eef2f5; border-radius: var(--radius); padding: 1rem; display: flex; flex-direction: column; }
        .column-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem; padding-bottom: 0.5rem; border-bottom: 2px solid rgba(0,0,0,0.05); }
        .column-header h4 { font-size: 0.95rem; text-transform: uppercase; letter-spacing: 0.5px; }
        .badge { background: #dfe6e9; padding: 2px 8px; border-radius: 10px; font-size: 0.8rem; }

        /* --- LEAD CARD --- */
        .lead-card { background: var(--surface); padding: 1rem; border-radius: 8px; margin-bottom: 0.8rem; box-shadow: 0 2px 4px rgba(0,0,0,0.03); cursor: grab; border: 1px solid transparent; transition: all 0.2s; position: relative; }
        .lead-card:hover { transform: translateY(-2px); box-shadow: var(--shadow); border-color: var(--primary); }
        .lead-card:active { cursor: grabbing; }
        .lead-card.dragging { opacity: 0.5; border: 2px dashed var(--primary); }
        
        .card-top { display: flex; justify-content: space-between; margin-bottom: 0.5rem; }
        .lead-name { font-weight: 600; font-size: 0.95rem; }
        .lead-budget { font-size: 0.8rem; color: var(--text-light); background: #f1f2f6; padding: 2px 6px; border-radius: 4px; }
        .lead-model { font-size: 0.85rem; color: var(--primary); margin-bottom: 0.5rem; display: block; font-weight: 600; }
        .contact-row { display: flex; align-items: center; gap: 0.8rem; font-size: 0.75rem; color: #636e72; margin-bottom: 0.5rem; }
        
        /* Action Buttons in Card */
        .card-actions { display: flex; gap: 0.5rem; margin-top: 10px; padding-top: 10px; border-top: 1px solid #f1f2f6; overflow-x: auto; }
        .card-actions button { flex-shrink: 0; }

        /* --- PROGRESS BAR --- */
        .progress-wrapper { margin-top: 0.5rem; }
        .progress-info { display: flex; justify-content: space-between; font-size: 0.8rem; margin-bottom: 4px; }
        .progress-bg { background: #e0e0e0; border-radius: 10px; height: 8px; width: 100%; overflow: hidden; cursor: pointer; }
        .progress-bg:hover { background: #d0d0d0; }
        .progress-fill { height: 100%; background: var(--success); transition: width 0.5s ease; }

        /* --- AI PANEL --- */
        .ai-panel { background: linear-gradient(135deg, #2d3436 0%, #072870 100%); color: white; padding: 1.5rem; border-radius: var(--radius); margin-bottom: 2rem; display: flex; align-items: center; justify-content: space-between; box-shadow: 0 10px 20px rgba(0,0,0,0.2); }
        .ai-content h2 { font-size: 1.2rem; margin-bottom: 0.5rem; display: flex; align-items: center; gap: 0.5rem; }
        .ai-content p { color: #dfe6e9; font-size: 0.9rem; }

        /* --- TABLES --- */
        .table-container { background: var(--surface); border-radius: var(--radius); padding: 1.5rem; box-shadow: var(--shadow); overflow-x: auto; }
        table { width: 100%; border-collapse: collapse; min-width: 800px; }
        th, td { padding: 1rem; text-align: left; border-bottom: 1px solid var(--border); font-size: 0.9rem; }
        th { font-weight: 600; color: var(--text-light); background-color: #f8f9fa; }
        .editable-goal { cursor: pointer; color: var(--primary); text-decoration: underline; }
        
        /* --- MODALS --- */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); display: none; justify-content: center; align-items: center; z-index: 100; }
        .modal { background: var(--surface); padding: 2rem; border-radius: var(--radius); width: 600px; max-width: 95%; max-height: 90vh; overflow-y: auto; position: relative; }
        .modal-header { display: flex; justify-content: space-between; margin-bottom: 1.5rem; padding-bottom: 1rem; border-bottom: 1px solid var(--border); }
        .close-modal { background: none; border: none; font-size: 1.5rem; cursor: pointer; color: var(--text-light); }
        
        .form-group { margin-bottom: 1rem; }
        .form-group label { display: block; margin-bottom: 0.5rem; font-size: 0.9rem; font-weight: 600; color: var(--text); }
        .form-group input, .form-group select, .form-group textarea { width: 100%; padding: 0.6rem; border: 1px solid var(--border); border-radius: 6px; font-size: 0.9rem; }

        /* --- QUOTE DESIGN --- */
        .quote-paper { background: white; padding: 2rem; border: 1px solid #ddd; font-family: 'Times New Roman', serif; margin-bottom: 1rem; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .quote-header { text-align: center; margin-bottom: 2rem; border-bottom: 2px solid #072870; padding-bottom: 1rem; }
        .quote-logo { font-weight: bold; font-size: 1.5rem; color: #072870; font-family: sans-serif; text-transform: uppercase; }
        .quote-row { display: flex; justify-content: space-between; margin-bottom: 0.5rem; }
        .quote-total { margin-top: 1rem; font-weight: bold; font-size: 1.1rem; text-align: right; border-top: 1px solid #ddd; padding-top: 0.5rem; }

        /* --- AI DIAGNOSTICS --- */
        .diagnostic-card { background: linear-gradient(135deg, #f8f9fa 0%, #e6e9ef 100%); border-left: 4px solid var(--primary); padding: 1.5rem; border-radius: 8px; margin-bottom: 1rem; }
        .diagnostic-title { font-weight: 700; color: var(--primary); margin-bottom: 0.5rem; display: flex; align-items: center; gap: 0.5rem; }

        /* --- COMMUNICATIONS --- */
        .comm-layout { display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; }
        .template-card { padding: 1rem; border: 1px solid var(--border); border-radius: 8px; margin-bottom: 1rem; cursor: pointer; transition: 0.2s; }
        .template-card:hover { border-color: var(--primary); background: #f0f4ff; }
        .template-title { font-weight: 600; color: var(--primary); }
        .template-preview { font-size: 0.85rem; color: var(--text-light); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

        /* --- IMPORT --- */
        .import-zone { border: 2px dashed var(--primary); border-radius: var(--radius); padding: 4rem; text-align: center; background: #f0f4ff; margin-top: 2rem; transition: 0.2s; }
        .import-zone:hover { background: #e1e8fa; }
        .import-zone i { font-size: 3rem; color: var(--primary); margin-bottom: 1rem; }

        /* --- TOAST --- */
        .toast-container { position: fixed; bottom: 20px; right: 20px; display: flex; flex-direction: column; gap: 10px; z-index: 200; }
        .toast { background: var(--surface); border-left: 4px solid var(--primary); padding: 1rem; border-radius: 4px; box-shadow: 0 4px 12px rgba(0,0,0,0.15); display: flex; align-items: center; gap: 10px; animation: slideIn 0.3s ease; min-width: 300px; }
        @keyframes slideIn { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }

        .hidden { display: none !important; }
    </style>
</head>
<body>

    <!-- Sidebar -->
    <aside>
        <div class="brand">
            <i class="ph ph-motorcycle"></i> EWE Store Tunja
        </div>
        <nav>
            <button class="active" onclick="showView('dashboard')">
                <i class="ph ph-squares-four"></i> Tablero
            </button>
            <button onclick="showView('advisors')">
                <i class="ph ph-users-three"></i> Asesor Comercial
            </button>
            <button onclick="showView('contacts')">
                <i class="ph ph-address-book"></i> Contactos
            </button>
            <button onclick="showView('history')">
                <i class="ph ph-clock-counter-clockwise"></i> Historias
            </button>
            <button onclick="showView('reports')">
                <i class="ph ph-chart-pie-slice"></i> Reporte Semanal
            </button>
            <button onclick="showView('communications')">
                <i class="ph ph-envelope-simple"></i> Comunicaciones
            </button>
            <button onclick="showView('import')">
                <i class="ph ph-upload-simple"></i> Importar
            </button>
            <button onclick="showView('automation')">
                <i class="ph ph-robot"></i> Automatización
            </button>
        </nav>
    </aside>

    <!-- Main Content -->
    <main id="main-content">
        
        <!-- VISTA: DASHBOARD -->
        <section id="view-dashboard">
            <header>
                <div>
                    <h1>Panel de Control</h1>
                    <p style="color: var(--text-light)">Resumen general y métricas</p>
                </div>
                <button class="btn btn-primary" onclick="openAddLeadModal()">
                    <i class="ph ph-plus"></i> Nuevo Lead
                </button>
            </header>

            <div class="ai-panel">
                <div class="ai-content">
                    <h2><i class="ph ph-sparkle"></i> AI Assistant</h2>
                    <p id="ai-recommendation">Sistema listo. Analizando datos en tiempo real...</p>
                </div>
            </div>

            <div class="stats-grid">
                <div class="stat-card">
                    <h3>Leads Hoy</h3>
                    <div class="value" id="stat-today">0</div>
                    <span class="trend up">Nuevos registros</span>
                </div>
                <div class="stat-card">
                    <h3>Leads Mes</h3>
                    <div class="value" id="stat-month">0</div>
                    <span class="trend">Acumulado mensual</span>
                </div>
                <div class="stat-card">
                    <h3>Pipeline</h3>
                    <div class="value" id="stat-pipeline">$0</div>
                    <span class="trend">Valor potencial</span>
                </div>
                <div class="stat-card">
                    <h3>Conversión</h3>
                    <div class="value" id="stat-conversion">0%</div>
                    <span class="trend up">Cerrados / Total</span>
                </div>
            </div>

            <div class="charts-grid">
                <div class="chart-card">
                    <canvas id="leadsChart"></canvas>
                </div>
                <div class="chart-card">
                    <canvas id="sourceChart"></canvas>
                </div>
            </div>

            <h2>Oportunidades Destacadas</h2>
            <div class="kanban-board" id="dashboard-kanban"></div>
        </section>

        <!-- VISTA: ASESOR COMERCIAL -->
        <section id="view-advisors" class="hidden">
            <header>
                <div>
                    <h1>Asesor Comercial</h1>
                    <p style="color: var(--text-light)">Gestión de desempeño y metas</p>
                </div>
                <button class="btn btn-primary" onclick="openAddAdvisorModal()"><i class="ph ph-user-plus"></i> Agregar Asesor</button>
            </header>

            <div class="kpi-grid">
                <div class="kpi-card">
                    <h3>Mejor Asesor</h3>
                    <div class="value" id="kpi-best-advisor">-</div>
                </div>
                <div class="kpi-card">
                    <h3>Ventas Totales</h3>
                    <div class="value" id="kpi-total-sales">$0</div>
                </div>
                <div class="kpi-card">
                    <h3>Cumplimiento Promedio</h3>
                    <div class="value" id="kpi-avg-achievement">0%</div>
                </div>
                <div class="kpi-card">
                    <h3>Leads Activos</h3>
                    <div class="value" id="kpi-active-leads">0</div>
                </div>
            </div>

            <div class="charts-grid">
                <div class="chart-card">
                    <canvas id="advisorSalesChart"></canvas>
                </div>
                <div class="chart-card">
                    <canvas id="advisorGoalChart"></canvas>
                </div>
            </div>

            <h2>Tabla de Desempeño</h2>
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Asesor</th>
                            <th>Asignados</th>
                            <th>Contactados</th>
                            <th>Ventas</th>
                            <th>Conversión</th>
                            <th>Meta Mensual (Clic para editar)</th>
                            <th>Acumulado</th>
                            <th>Cumplimiento</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody id="advisors-table-body">
                        <!-- JS Fill -->
                    </tbody>
                </table>
            </div>
        </section>

        <!-- VISTA: CONTACTOS -->
        <section id="view-contacts" class="hidden">
            <header>
                <div>
                    <h1>Gestión de Contactos</h1>
                </div>
                <button class="btn btn-primary" onclick="openAddLeadModal()">
                    <i class="ph ph-plus"></i> Nuevo Lead
                </button>
            </header>
            <h2>Oportunidades Destacadas</h2>
            <div class="kanban-board" id="contacts-kanban"></div>
        </section>

        <!-- VISTA: HISTORIAS -->
        <section id="view-history" class="hidden">
            <header><h1>Historias de Interacciones</h1></header>
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Cliente</th>
                            <th>Fase</th>
                            <th>Origen</th>
                            <th>Días sin contacto</th>
                            <th>Última interacción</th>
                            <th>Acciones</th>
                        </tr>
                    </thead>
                    <tbody id="history-table-body"></tbody>
                </table>
            </div>
        </section>

        <!-- VISTA: REPORTE SEMANAL -->
        <section id="view-reports" class="hidden">
            <header><h1>Reporte Semanal</h1></header>

            <h3><i class="ph ph-magic-wand"></i> Pre-Diagnóstico Inteligente</h3>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1rem; margin-bottom: 2rem;">
                <div class="diagnostic-card">
                    <div class="diagnostic-title"><i class="ph ph-chart-bar"></i> Fuentes de Leads</div>
                    <p id="diag-source">Analizando fuentes...</p>
                </div>
                <div class="diagnostic-card">
                    <div class="diagnostic-title"><i class="ph ph-trend-up"></i> Tasa de Conversión</div>
                    <p id="diag-conversion">Calculando conversión...</p>
                </div>
                <div class="diagnostic-card">
                    <div class="diagnostic-title"><i class="ph ph-clock"></i> Tiempo de Respuesta</div>
                    <p id="diag-time">Promedio de contacto...</p>
                </div>
                <div class="diagnostic-card">
                    <div class="diagnostic-title"><i class="ph ph-funnel"></i> Embudo Crítico</div>
                    <p id="diag-bottleneck">Fase más lenta detectada...</p>
                </div>
            </div>

            <div class="charts-grid">
                <div class="chart-card">
                    <canvas id="funnelChart"></canvas>
                </div>
                <div class="chart-card">
                    <canvas id="weeklyActivityChart"></canvas>
                </div>
            </div>

            <h3>Actividad por Asesor</h3>
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Asesor</th>
                            <th>Llamadas</th>
                            <th>Reuniones</th>
                            <th>Ventas</th>
                            <th>Conversión Total</th>
                        </tr>
                    </thead>
                    <tbody id="report-activity-body"></tbody>
                </table>
            </div>
        </section>

        <!-- VISTA: COMUNICACIONES -->
        <section id="view-communications" class="hidden">
            <header>
                <div><h1>Comunicaciones</h1></div>
                <button class="btn btn-primary" onclick="openEmailModal()"><i class="ph ph-paper-plane-tilt"></i> Nuevo Email</button>
            </header>

            <div class="comm-layout">
                <div>
                    <h3 style="margin-bottom:1rem;">Plantillas de Email</h3>
                    <div id="templates-container"></div>
                </div>
                <div>
                    <h3 style="margin-bottom:1rem;">💡 IA Sugestiones</h3>
                    <div id="ai-suggestions-container"></div>
                </div>
            </div>
        </section>

        <!-- VISTA: IMPORTAR -->
        <section id="view-import" class="hidden">
            <header><h1>Importar Leads</h1></header>
            <div class="import-zone" id="drop-zone">
                <i class="ph ph-file-arrow-up"></i>
                <h3>Arrastra tu archivo aquí</h3>
                <input type="file" id="file-input" accept=".csv, .xlsx, .pdf" style="display:none">
                <button class="btn btn-primary" onclick="document.getElementById('file-input').click()">Seleccionar Archivo</button>
            </div>
        </section>

        <!-- VISTA: AUTOMATIZACIÓN -->
        <section id="view-automation" class="hidden">
            <header><h1>Automatización</h1></header>
            <div class="stats-grid">
                <div class="stat-card">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem;">
                        <h3><i class="ph ph-whatsapp-logo" style="color: #25D366;"></i> Bienvenida</h3>
                        <label><input type="checkbox" checked onchange="toggleAutomation('whatsapp', this.checked)"> Activo</label>
                    </div>
                    <p>Mensaje automático a leads nuevos.</p>
                </div>
                <div class="stat-card">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem;">
                        <h3><i class="ph ph-clock-warning" style="color: var(--warning);"></i> Lead sin contacto</h3>
                        <label><input type="checkbox" checked onchange="toggleAutomation('no-contact', this.checked)"> Activo</label>
                    </div>
                    <p>Alerta si un lead lleva más de 3 días sin ser contactado.</p>
                </div>
                <div class="stat-card">
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem;">
                        <h3><i class="ph ph-lightning" style="color: var(--warning);"></i> Lead Caliente</h3>
                        <label><input type="checkbox" checked onchange="toggleAutomation('hot-lead', this.checked)"> Activo</label>
                    </div>
                    <p>Alerta de IA para leads con score > 85.</p>
                </div>
            </div>
        </section>
    </main>

    <!-- MODAL: ADD/EDIT LEAD -->
    <div class="modal-overlay" id="add-lead-modal">
        <div class="modal">
            <div class="modal-header">
                <h2 id="modal-title-lead">Agregar Nuevo Lead</h2>
                <button class="close-modal" onclick="closeAddLeadModal()">&times;</button>
            </div>
            <form id="add-lead-form" onsubmit="handleAddLead(event)">
                <input type="hidden" id="lead-id" name="id">
                
                <div class="form-group">
                    <label>Nombre Completo</label>
                    <input type="text" id="lead-name" name="name" required placeholder="Ej: Juan Pérez">
                </div>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
                    <div class="form-group">
                        <label>Teléfono</label>
                        <input type="tel" id="lead-phone" name="phone" placeholder="Ej: 300 123 4567">
                    </div>
                    <div class="form-group">
                        <label>Correo</label>
                        <input type="email" id="lead-email" name="email" placeholder="ejemplo@email.com">
                    </div>
                </div>

                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
                    <div class="form-group">
                        <label>Fecha de Creación</label>
                        <input type="date" id="lead-created-at" name="createdAt" required>
                    </div>
                    <div class="form-group">
                        <label>¿Dónde nos conoció?</label>
                        <select id="lead-source" name="source">
                            <option value="instagram">Instagram</option>
                            <option value="facebook">Facebook</option>
                            <option value="whatsapp">WhatsApp</option>
                            <option value="google">Google / Maps</option>
                            <option value="store">Tienda Física</option>
                            <option value="other">Otro</option>
                        </select>
                    </div>
                </div>

                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
                    <div class="form-group">
                        <label>Asesor Comercial</label>
                        <select id="lead-advisor" name="advisor"></select>
                    </div>
                    <div class="form-group">
                        <label>Estado</label>
                        <select id="lead-status" name="status">
                            <option value="new">Nuevo</option>
                            <option value="contacted">Contactado</option>
                            <option value="negotiating">Negociación</option>
                        </select>
                    </div>
                </div>
                <div class="form-group">
                    <label>Modelo</label>
                    <select id="lead-model" name="model">
                        <option value="TRX">TRX</option>
                        <option value="Strong">Strong</option>
                        <option value="Bee">Bee</option>
                        <option value="Raptor">Raptor</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Presupuesto (COP)</label>
                    <input type="number" id="lead-budget" name="budget" required placeholder="Ej: 3500000">
                </div>
                <div class="form-group">
                    <label>Observaciones</label>
                    <textarea id="lead-observations" name="observations" rows="3"></textarea>
                </div>
                <div style="display: flex; gap: 1rem; justify-content: flex-end; margin-top: 1.5rem;">
                    <button type="button" class="btn btn-outline" onclick="closeAddLeadModal()">Cancelar</button>
                    <button type="submit" class="btn btn-primary">Guardar</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL: ADD ADVISOR -->
    <div class="modal-overlay" id="add-advisor-modal">
        <div class="modal">
            <div class="modal-header">
                <h2>Nuevo Asesor</h2>
                <button class="close-modal" onclick="document.getElementById('add-advisor-modal').style.display='none'">&times;</button>
            </div>
            <form onsubmit="handleAddAdvisor(event)">
                <div class="form-group">
                    <label>Nombre del Asesor</label>
                    <input type="text" id="advisor-name" required>
                </div>
                <div class="form-group">
                    <label>Meta Mensual (COP)</label>
                    <input type="number" id="advisor-goal" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width: 100%">Guardar Asesor</button>
            </form>
        </div>
    </div>

    <!-- MODAL: INTERACCIONES -->
    <div class="modal-overlay" id="interaction-modal">
        <div class="modal">
            <div class="modal-header">
                <h2>Registrar Interacción</h2>
                <button class="close-modal" onclick="closeInteractionModal()">&times;</button>
            </div>
            <form onsubmit="handleInteraction(event)">
                <input type="hidden" id="interaction-lead-id">
                <div class="form-group">
                    <label>Tipo</label>
                    <select id="interaction-type">
                        <option value="Llamada">Llamada</option>
                        <option value="Reunión">Reunión</option>
                        <option value="Cotización">Cotización</option>
                        <option value="Email">Email</option>
                        <option value="Nota">Nota Interna</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Descripción / Notas</label>
                    <textarea id="interaction-note" rows="3" required placeholder="Detalles de la interacción..."></textarea>
                </div>
                <button type="submit" class="btn btn-primary" style="width: 100%">Guardar Interacción</button>
            </form>
            
            <h3 style="margin-top: 2rem; margin-bottom: 1rem; font-size: 1rem;">Historial Reciente</h3>
            <div id="interaction-history-list" style="max-height: 200px; overflow-y: auto;"></div>
        </div>
    </div>

    <!-- MODAL: COTIZACIÓN -->
    <div class="modal-overlay" id="quote-modal">
        <div class="modal">
            <div class="modal-header">
                <h2>Generar Cotización</h2>
                <button class="close-modal" onclick="document.getElementById('quote-modal').style.display='none'">&times;</button>
            </div>
            <div class="quote-paper" id="quote-preview">
                <div class="quote-header">
                    <div class="quote-logo">EWE STORE TUNJA</div>
                    <div>Cotización Oficial</div>
                </div>
                <div class="quote-row">
                    <span><strong>Cliente:</strong> <span id="quote-client-name">Nombre Cliente</span></span>
                    <span><strong>Fecha:</strong> <span id="quote-date">DD/MM/AAAA</span></span>
                </div>
                <br>
                <div class="quote-row" style="border-bottom: 1px solid #eee; padding-bottom: 5px;">
                    <strong>Producto</strong>
                    <strong>Valor Unitario</strong>
                    <strong>Total</strong>
                </div>
                <div class="quote-row">
                    <span id="quote-product">Modelo X</span>
                    <span id="quote-price">$0</span>
                    <span id="quote-total">$0</span>
                </div>
                <br>
                <div style="font-size: 0.9rem; margin-bottom: 1rem;">
                    <strong>Observaciones:</strong> <span id="quote-obs">Sin observaciones.</span>
                </div>
                <div class="quote-total">
                    TOTAL: <span id="quote-final-total">$0</span>
                </div>
            </div>
            <button class="btn btn-primary" style="width: 100%" onclick="downloadQuotePDF()">
                <i class="ph ph-file-pdf"></i> Descargar PDF
            </button>
        </div>
    </div>

    <!-- MODAL: EMAIL -->
    <div class="modal-overlay" id="email-modal">
        <div class="modal">
            <div class="modal-header">
                <h2>Enviar Email</h2>
                <button class="close-modal" onclick="document.getElementById('email-modal').style.display='none'">&times;</button>
            </div>
            <form onsubmit="handleSendEmail(event)">
                <div class="form-group">
                    <label>Para (Cliente)</label>
                    <input type="text" id="email-to" readonly style="background: #f0f0f0;">
                </div>
                <div class="form-group">
                    <label>Asunto</label>
                    <input type="text" id="email-subject" required>
                </div>
                <div class="form-group">
                    <label>Mensaje</label>
                    <textarea id="email-body" rows="5" required></textarea>
                </div>
                <button type="submit" class="btn btn-primary" style="width: 100%">Enviar Email</button>
            </form>
        </div>
    </div>

    <!-- Toast Container -->
    <div class="toast-container" id="toast-container"></div>

    <script>
        // --- DATA ---
        const formatCurrency = (amount) => {
            return new Intl.NumberFormat('es-CO', { style: 'currency', currency: 'COP', maximumFractionDigits: 0 }).format(amount);
        };

        // Base de datos vacía de leads
        let leads = [];

        // Configuración de Asesores
        let advisors = [
            { id: 1, name: "Carlos Pérez", goal: 20000000, color: "#072870" },
            { id: 2, name: "Maria Gomez", goal: 25000000, color: "#0984e3" },
            { id: 3, name: "Jorge Torres", goal: 18000000, color: "#00b894" }
        ];

        const columns = [
            { id: 'new', title: 'Nuevo' },
            { id: 'contacted', title: 'Contactado' },
            { id: 'negotiating', title: 'Negociación' },
            { id: 'won', title: 'Venta Cerrada' },
            { id: 'lost', title: 'Perdido' }
        ];

        const automations = { whatsapp: true, hotLead: true, noContact: true };
        
        // Plantillas de Email
        const emailTemplates = [
            { id: 1, title: "Bienvenida", subject: "¡Bienvenido a EWE Store Tunja!", body: "Hola [Nombre], gracias por tu interés en nuestras motos eléctricas. Estamos listos para asesorarte." },
            { id: 2, title: "Seguimiento", subject: "¿Cómo podemos ayudarte?", body: "Hola [Nombre], queríamos saber si sigues interesado en el modelo [Modelo]." },
            { id: 3, title: "Cotización Enviada", subject: "Tu cotización está lista", body: "Hola [Nombre], adjunto la cotización que solicitaste. Quedamos atentos." },
            { id: 4, title: "Recordatorio", subject: "Recordatorio de reunión", body: "Hola [Nombre], te recordamos nuestra reunión agendada para el día [Fecha]." },
            { id: 5, title: "Cliente Inactivo", subject: "¿Sigues ahí?", body: "Hola [Nombre], notamos que hemos perdido contacto. ¿Hay algo nuevo sobre tu compra?" }
        ];

        // Charts Instances
        let lineChartInstance = null;
        let pieChartInstance = null;
        let advisorSalesInstance = null;
        let advisorGoalInstance = null;
        let funnelInstance = null;
        let weeklyActivityInstance = null;

        // --- CORE FUNCTIONS ---

        function init() {
            renderStats();
            renderCharts();
            renderKanban('dashboard-kanban');
            renderKanban('contacts-kanban');
            renderHistory();
            renderAdvisors();
            renderReports();
            renderCommunications();
            setupDragAndDrop();
            setupFileImport();
            
            // Populate advisor select in modal
            updateAdvisorSelect();
            
            // Set default date
            const dateInput = document.getElementById('lead-created-at');
            if(dateInput) dateInput.valueAsDate = new Date();
        }

        function updateAdvisorSelect() {
            const advisorSelect = document.getElementById('lead-advisor');
            if(advisorSelect) {
                advisorSelect.innerHTML = '<option value="">Sin asignar</option>';
                advisors.forEach(adv => {
                    advisorSelect.innerHTML += `<option value="${adv.name}">${adv.name}</option>`;
                });
            }
        }

        function showView(viewName) {
            document.querySelectorAll('main > section').forEach(el => el.classList.add('hidden'));
            document.getElementById(`view-${viewName}`).classList.remove('hidden');
            document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));
            event.currentTarget.classList.add('active');
            
            if(viewName === 'dashboard') {
                renderStats();
                renderCharts();
            } else if (viewName === 'advisors') {
                renderAdvisors();
            } else if (viewName === 'history') {
                renderHistory();
            } else if (viewName === 'reports') {
                renderReports();
            }
        }

        // --- RENDER: ASESOR COMERCIAL ---
        function renderAdvisors() {
            const tbody = document.getElementById('advisors-table-body');
            tbody.innerHTML = '';
            let bestAdvisor = { name: '-', sales: 0 };
            let totalSales = 0;
            let totalAchievementSum = 0;

            advisors.forEach(adv => {
                const advLeads = leads.filter(l => l.advisor === adv.name);
                const advSales = advLeads.filter(l => l.status === 'won');
                const advTotalValue = advSales.reduce((sum, l) => sum + l.budget, 0);
                
                // Calcular conversion de este asesor
                const conversion = advLeads.length ? Math.round((advSales.length / advLeads.length) * 100) : 0;
                const achievement = adv.goal ? Math.round((advTotalValue / adv.goal) * 100) : 0;

                totalSales += advTotalValue;
                totalAchievementSum += achievement;

                if (advTotalValue > bestAdvisor.sales) bestAdvisor = { name: adv.name, sales: advTotalValue };

                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td><strong>${adv.name}</strong></td>
                    <td>${advLeads.length}</td>
                    <td>${advLeads.filter(l => l.status === 'contacted' || l.status === 'negotiating').length}</td>
                    <td>${advSales.length}</td>
                    <td>${conversion}%</td>
                    <td>
                        <span class="editable-goal" onclick="editAdvisorGoal(${adv.id})" title="Click para editar meta">
                            ${formatCurrency(adv.goal)}
                        </span>
                    </td>
                    <td>${formatCurrency(advTotalValue)}</td>
                    <td>
                        <div class="progress-wrapper">
                            <div class="progress-info">
                                <span>${achievement}%</span>
                            </div>
                            <div class="progress-bg">
                                <div class="progress-fill" style="width: ${Math.min(achievement, 100)}%; background-color: ${achievement >= 100 ? 'var(--success)' : 'var(--primary)'}"></div>
                            </div>
                        </div>
                    </td>
                    <td>
                        <button class="btn btn-danger btn-sm" onclick="deleteAdvisor(${adv.id})">Eliminar</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });

            document.getElementById('kpi-best-advisor').innerText = bestAdvisor.name;
            document.getElementById('kpi-total-sales').innerText = formatCurrency(totalSales);
            document.getElementById('kpi-avg-achievement').innerText = advisors.length ? Math.round(totalAchievementSum / advisors.length) + "%" : "0%";
            document.getElementById('kpi-active-leads').innerText = leads.length;

            renderAdvisorCharts(advisors);
        }

        function renderAdvisorCharts(data) {
            // Sales Chart
            const ctxSales = document.getElementById('advisorSalesChart').getContext('2d');
            const salesData = data.map(adv => leads.filter(l => l.advisor === adv.name && l.status === 'won').reduce((sum, l) => sum + l.budget, 0));
            
            if(advisorSalesInstance) advisorSalesInstance.destroy();
            advisorSalesInstance = new Chart(ctxSales, {
                type: 'bar',
                data: {
                    labels: data.map(d => d.name),
                    datasets: [{ label: 'Ventas', data: salesData, backgroundColor: '#072870' }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: {display: false}, title: {display: true, text: 'Ventas por Asesor'} } }
            });

            // Goal Chart
            const ctxGoal = document.getElementById('advisorGoalChart').getContext('2d');
            const goalData = data.map(adv => {
                const current = leads.filter(l => l.advisor === adv.name && l.status === 'won').reduce((sum, l) => sum + l.budget, 0);
                return (current / adv.goal) * 100;
            });

            if(advisorGoalInstance) advisorGoalInstance.destroy();
            advisorGoalInstance = new Chart(ctxGoal, {
                type: 'radar',
                data: {
                    labels: data.map(d => d.name),
                    datasets: [{ label: 'Cumplimiento %', data: goalData, backgroundColor: 'rgba(7, 40, 112, 0.2)', borderColor: '#072870', pointBackgroundColor: '#072870' }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: {display: false}, title: {display: true, text: 'Cumplimiento de Metas'} } }
            });
        }

        // --- RENDER: REPORTES ---
        function renderReports() {
            // AI Diagnostics Logic
            const sourceCounts = {};
            const statusCounts = { new: 0, contacted: 0, negotiating: 0, won: 0, lost: 0 };
            let totalDays = 0;
            let contactCount = 0;

            leads.forEach(l => {
                sourceCounts[l.source] = (sourceCounts[l.source] || 0) + 1;
                statusCounts[l.status] = statusCounts[l.status] + 1;
                if(l.interactions && l.interactions.length > 0) { }
            });

            const bestSource = Object.keys(sourceCounts).sort((a,b) => sourceCounts[b] - sourceCounts[a])[0] || "N/A";
            const bottleneck = Object.keys(statusCounts).sort((a,b) => statusCounts[b] - statusCounts[a])[0]; 

            document.getElementById('diag-source').innerText = `${bestSource} genera la mayoría de los leads (${sourceCounts[bestSource] || 0}).`;
            document.getElementById('diag-conversion').innerText = `Tasa general: ${leads.length ? Math.round((statusCounts.won/leads.length)*100) : 0}%.`;
            document.getElementById('diag-bottleneck').innerText = `La fase crítica es "${columns.find(c=>c.id===bottleneck)?.title.toUpperCase()}" con ${statusCounts[bottleneck]} leads acumulados.`;
            document.getElementById('diag-time').innerText = "Tiempo promedio de primer contacto: 4 horas (Estimado).";

            // Charts
            const ctxFunnel = document.getElementById('funnelChart').getContext('2d');
            if(funnelInstance) funnelInstance.destroy();
            funnelInstance = new Chart(ctxFunnel, {
                type: 'bar',
                data: {
                    labels: columns.map(c => c.title),
                    datasets: [{
                        label: 'Leads',
                        data: columns.map(c => leads.filter(l => l.status === c.id).length),
                        backgroundColor: ['#072870', '#0984e3', '#fdcb6e', '#00b894', '#d63031']
                    }]
                },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: {display: false}, title: {display: true, text: 'Embudo de Ventas'} } }
            });

            // Activity Table
            const reportBody = document.getElementById('report-activity-body');
            reportBody.innerHTML = '';
            advisors.forEach(adv => {
                const advLeads = leads.filter(l => l.advisor === adv.name);
                const interactions = advLeads.reduce((acc, l) => acc + (l.interactions ? l.interactions.length : 0), 0);
                const won = advLeads.filter(l => l.status === 'won').length;
                const conv = advLeads.length ? Math.round((won/advLeads.length)*100) : 0;

                reportBody.innerHTML += `
                    <tr>
                        <td>${adv.name}</td>
                        <td>${interactions}</td>
                        <td>${Math.floor(interactions * 0.4)}</td>
                        <td>${won}</td>
                        <td>${conv}%</td>
                    </tr>
                `;
            });
        }

        // --- RENDER: COMUNICACIONES ---
        function renderCommunications() {
            const container = document.getElementById('templates-container');
            container.innerHTML = '';
            emailTemplates.forEach(t => {
                const div = document.createElement('div');
                div.className = 'template-card';
                div.onclick = () => loadEmailTemplate(t);
                div.innerHTML = `
                    <div class="template-title">${t.title}</div>
                    <div class="template-preview">${t.subject} - ${t.body.substring(0, 40)}...</div>
                `;
                container.appendChild(div);
            });

            // AI Suggestions
            const aiContainer = document.getElementById('ai-suggestions-container');
            aiContainer.innerHTML = '';
            const suggestions = [
                "Hola [Nombre], notamos tu interés en la moto [Modelo]. ¿Tienes alguna duda específica?",
                "Adjuntamos la cotización detallada para el modelo [Modelo] que solicitaste.",
                "Hace unos días nos contactaste. Queremos saber si seguimos en carrera para la compra.",
                "Recordatorio: Tu cita de prueba está programada para el [Día] a las [Hora]."
            ];
            suggestions.forEach(s => {
                const div = document.createElement('div');
                div.className = 'template-card';
                div.style.cursor = 'default';
                div.innerHTML = `<div style="font-size:0.8rem; color: #555;">💡 ${s}</div>`;
                aiContainer.appendChild(div);
            });
        }

        function loadEmailTemplate(template) {
            openEmailModal();
            document.getElementById('email-subject').value = template.subject;
            document.getElementById('email-body').value = template.body;
            showToast("Plantilla cargada", "info");
        }

        function openEmailModal(leadEmail = "") {
            document.getElementById('email-modal').style.display = 'flex';
            document.getElementById('email-to').value = leadEmail || "cliente@ejemplo.com";
        }

        function handleSendEmail(e) {
            e.preventDefault();
            document.getElementById('email-modal').style.display = 'none';
            showToast("Email enviado correctamente", "success");
        }

        // --- RENDER STATS ---
        function renderStats() {
            const today = new Date().toISOString().split('T')[0];
            const leadsToday = leads.filter(l => l.createdAt === today).length;
            const currentMonth = new Date().getMonth();
            const leadsMonth = leads.filter(l => new Date(l.createdAt).getMonth() === currentMonth).length;
            const totalValue = leads.reduce((sum, lead) => sum + lead.budget, 0);
            const conversionRate = leads.length ? Math.round((leads.filter(l => l.status === 'won').length / leads.length) * 100) : 0;

            document.getElementById('stat-today').innerText = leadsToday;
            document.getElementById('stat-month').innerText = leadsMonth;
            document.getElementById('stat-pipeline').innerText = formatCurrency(totalValue);
            document.getElementById('stat-conversion').innerText = conversionRate + "%";
        }

        function renderCharts() {
            // Line Chart
            const ctxLine = document.getElementById('leadsChart').getContext('2d');
            const labelsLine = [];
            const dataLine = [];
            for (let i = 29; i >= 0; i--) {
                const d = new Date(); d.setDate(d.getDate() - i);
                labelsLine.push(d.getDate() + '/' + (d.getMonth() + 1));
                dataLine.push(Math.floor(Math.random() * 10)); 
            }
            if (lineChartInstance) lineChartInstance.destroy();
            lineChartInstance = new Chart(ctxLine, {
                type: 'line', data: { labels: labelsLine, datasets: [{ label: 'Leads por día', data: dataLine, borderColor: '#072870', backgroundColor: 'rgba(7, 40, 112, 0.1)', borderWidth: 2, tension: 0.4, fill: true }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false }, title: { display: true, text: 'Últimos 30 días' } } }
            });

            // Pie Chart (Sources)
            const sourceCounts = { instagram: 0, facebook: 0, whatsapp: 0, google: 0, store: 0, other: 0 };
            leads.forEach(l => { if(sourceCounts[l.source] !== undefined) sourceCounts[l.source]++; });
            const ctxPie = document.getElementById('sourceChart').getContext('2d');
            if (pieChartInstance) pieChartInstance.destroy();
            pieChartInstance = new Chart(ctxPie, {
                type: 'doughnut', data: { labels: ['Instagram', 'Facebook', 'WhatsApp', 'Google', 'Tienda'], datasets: [{ data: [sourceCounts.instagram, sourceCounts.facebook, sourceCounts.whatsapp, sourceCounts.google, sourceCounts.store], backgroundColor: ['#C13584', '#4267B2', '#25D366', '#DB4437', '#072870'], borderWidth: 1 }] },
                options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'bottom' }, title: { display: true, text: 'Fuentes de Contacto' } } }
            });
        }

        function renderHistory() {
            const tbody = document.getElementById('history-table-body');
            tbody.innerHTML = '';
            if(leads.length === 0) { tbody.innerHTML = '<tr><td colspan="6" style="text-align:center; padding:2rem;">No hay registros</td></tr>'; return; }
            
            leads.forEach(lead => {
                let lastInter = lead.interactions && lead.interactions.length > 0 ? lead.interactions[lead.interactions.length-1] : null;
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td><strong>${lead.name}</strong></td>
                    <td>${columns.find(c=>c.id===lead.status)?.title}</td>
                    <td>${lead.source}</td>
                    <td>Días sin contacto: ${Math.floor(Math.random()*5)}</td>
                    <td>${lastInter ? `${lastInter.date} - ${lastInter.type} por ${lastInter.user}` : 'Sin actividad'}</td>
                    <td><button class="btn btn-sm btn-outline" onclick="openInteractionModal(${lead.id})">Ver/Registrar</button></td>
                `;
                tbody.appendChild(tr);
            });
        }

        function renderKanban(containerId) {
            const board = document.getElementById(containerId);
            if(!board) return; board.innerHTML = '';

            columns.forEach(col => {
                const colDiv = document.createElement('div');
                colDiv.className = 'kanban-column';
                colDiv.setAttribute('data-status', col.id);
                const colLeads = leads.filter(l => l.status === col.id);
                let cardsHTML = '';

                colLeads.forEach(lead => {
                    let scoreClass = lead.score > 80 ? 'score-high' : (lead.score > 50 ? 'score-med' : 'score-low');
                    let iconClass = 'ph-globe';
                    if(lead.source === 'instagram') iconClass = 'ph-instagram-logo';
                    if(lead.source === 'facebook') iconClass = 'ph-facebook-logo';
                    if(lead.source === 'whatsapp') iconClass = 'ph-whatsapp-logo';
                    
                    cardsHTML += `
                        <div class="lead-card" draggable="true" data-id="${lead.id}">
                            <div class="card-top">
                                <span class="lead-name">${lead.name}</span>
                                <span class="lead-budget">${formatCurrency(lead.budget)}</span>
                            </div>
                            <span class="lead-model">${lead.model}</span>
                            <div class="contact-row">
                                <span><i class="ph ${iconClass}"></i> ${lead.source}</span>
                                <span><i class="ph ph-user"></i> ${lead.advisor || '-'}</span>
                            </div>
                            
                            <!-- Corrección 1: IA Recomendación al lado del score -->
                            <div style="font-size: 0.75rem; color: #555; margin-top: 5px; padding: 5px; background: #f8f9fa; border-radius: 4px;">
                                <strong>IA Score: ${lead.score}</strong> - ${lead.aiAction}
                            </div>
                            
                            <div class="card-actions">
                                <button class="btn-icon" title="Editar" onclick="editLead(${lead.id})"><i class="ph ph-pencil-simple"></i></button>
                                <button class="btn-icon" title="Eliminar" onclick="deleteLead(${lead.id})" style="color: var(--danger)"><i class="ph ph-trash"></i></button>
                                <button class="btn-icon" title="Generar Cotización" onclick="openQuoteModal(${lead.id})"><i class="ph ph-file-text"></i></button>
                                <button class="btn-icon" title="Registrar Interacción" onclick="openInteractionModal(${lead.id})"><i class="ph ph-phone"></i></button>
                                <button class="btn-icon" title="Enviar Email" onclick="openEmailModal('${lead.email || ''}')"><i class="ph ph-envelope"></i></button>
                            </div>
                        </div>
                    `;
                });

                colDiv.innerHTML = `
                    <div class="column-header"><h4>${col.title}</h4><span class="badge">${colLeads.length}</span></div>
                    <div class="cards-container">${cardsHTML}</div>
                `;
                board.appendChild(colDiv);
            });
            setupDragAndDrop();
        }

        // --- DRAG & DROP ---
        let draggedId = null;
        function setupDragAndDrop() {
            const cards = document.querySelectorAll('.lead-card');
            const columns = document.querySelectorAll('.kanban-column');
            cards.forEach(card => {
                card.addEventListener('dragstart', (e) => { draggedId = parseInt(e.target.getAttribute('data-id')); e.target.classList.add('dragging'); });
                card.addEventListener('dragend', (e) => { e.target.classList.remove('dragging'); draggedId = null; });
            });
            columns.forEach(col => {
                col.addEventListener('dragover', (e) => e.preventDefault());
                col.addEventListener('drop', (e) => {
                    e.preventDefault();
                    const newStatus = col.getAttribute('data-status');
                    if (draggedId) moveLead(draggedId, newStatus);
                });
            });
        }

        function moveLead(id, newStatus) {
            const lead = leads.find(l => l.id === id);
            if (lead && lead.status !== newStatus) {
                lead.status = newStatus;
                runAIScoring(lead);
                renderKanban('dashboard-kanban');
                renderKanban('contacts-kanban');
                renderStats();
                showToast(`Lead movido a ${columns.find(c=>c.id===newStatus).title}`, 'success');
            }
        }

        // --- LEAD LOGIC ---
        function runAIScoring(lead) {
            let score = 50;
            if(lead.budget > 4000000) score += 20;
            if(lead.status === 'negotiating') score += 30;
            lead.score = score;
            // Recomendaciones simples
            if (score > 90) lead.aiAction = "Prioridad Alta - Llamar Ya";
            else if (score > 75) lead.aiAction = "Enviar cotización detallada";
            else if (lead.status === 'new') lead.aiAction = "Enviar video de presentación";
            else lead.aiAction = "Seguimiento pasivo";
        }

        function openAddLeadModal() {
            document.getElementById('add-lead-modal').style.display = 'flex';
            document.getElementById('modal-title-lead').innerText = "Agregar Nuevo Lead";
            document.getElementById('lead-id').value = "";
            document.getElementById('add-lead-form').reset();
            const dateInput = document.getElementById('lead-created-at');
            if(dateInput) dateInput.valueAsDate = new Date();
        }

        function closeAddLeadModal() {
            document.getElementById('add-lead-modal').style.display = 'none';
        }

        function editLead(id) {
            const lead = leads.find(l => l.id === id);
            if(lead) {
                openAddLeadModal();
                document.getElementById('modal-title-lead').innerText = "Editar Lead";
                document.getElementById('lead-id').value = lead.id;
                document.getElementById('lead-name').value = lead.name;
                document.getElementById('lead-phone').value = lead.phone;
                document.getElementById('lead-email').value = lead.email;
                document.getElementById('lead-created-at').value = lead.createdAt;
                document.getElementById('lead-source').value = lead.source;
                document.getElementById('lead-advisor').value = lead.advisor;
                document.getElementById('lead-status').value = lead.status;
                document.getElementById('lead-model').value = lead.model;
                document.getElementById('lead-budget').value = lead.budget;
                document.getElementById('lead-observations').value = lead.observations;
            }
        }

        function deleteLead(id) {
            if(confirm("¿Seguro que deseas eliminar este lead?")) {
                leads = leads.filter(l => l.id !== id);
                renderKanban('dashboard-kanban');
                renderKanban('contacts-kanban');
                renderStats();
                showToast("Lead eliminado", "info");
            }
        }

        function handleAddLead(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            const id = document.getElementById('lead-id').value;
            
            const leadData = {
                name: formData.get('name'),
                phone: formData.get('phone'),
                email: formData.get('email'),
                createdAt: formData.get('createdAt'),
                source: formData.get('source'),
                advisor: formData.get('advisor'),
                status: formData.get('status'),
                model: formData.get('model'),
                budget: parseInt(formData.get('budget')),
                observations: formData.get('observations'),
                score: 50,
                interactions: []
            };

            if (id) {
                // Edit
                const index = leads.findIndex(l => l.id == id);
                if(index !== -1) {
                    leads[index] = { ...leads[index], ...leadData };
                    showToast("Lead actualizado", "success");
                }
            } else {
                // New
                leadData.id = Date.now();
                runAIScoring(leadData);
                leads.push(leadData);
                showToast("Lead creado", "success");
            }

            closeAddLeadModal();
            renderKanban('dashboard-kanban');
            renderKanban('contacts-kanban');
            renderStats();
        }

        // --- INTERACTIONS ---
        function openInteractionModal(leadId) {
            document.getElementById('interaction-modal').style.display = 'flex';
            document.getElementById('interaction-lead-id').value = leadId;
            renderInteractionList(leadId);
        }
        function closeInteractionModal() {
            document.getElementById('interaction-modal').style.display = 'none';
        }
        function renderInteractionList(leadId) {
            const lead = leads.find(l => l.id == leadId);
            const list = document.getElementById('interaction-history-list');
            list.innerHTML = '';
            if(!lead.interactions) return;
            
            // Mostrar de más reciente a más antigua
            [...lead.interactions].reverse().forEach(int => {
                list.innerHTML += `<div style="padding: 0.5rem; border-bottom: 1px solid #eee; font-size: 0.9rem;">
                    <strong>${int.date}</strong> - ${int.type}: ${int.note}<br>
                    <span style="font-size: 0.8rem; color: #888;">Por: ${int.user}</span>
                </div>`;
            });
        }
        function handleInteraction(e) {
            e.preventDefault();
            const id = document.getElementById('interaction-lead-id').value;
            const lead = leads.find(l => l.id == id);
            if(lead) {
                const type = document.getElementById('interaction-type').value;
                const note = document.getElementById('interaction-note').value;
                lead.interactions.push({ 
                    date: new Date().toLocaleDateString(), 
                    type: type, 
                    note: note,
                    user: "EWE Store" // Simulación de usuario logueado
                });
                renderInteractionList(id);
                showToast("Interacción registrada", "success");
                e.target.reset();
            }
        }

        // --- QUOTES ---
        function openQuoteModal(leadId) {
            const lead = leads.find(l => l.id === leadId);
            if(lead) {
                document.getElementById('quote-modal').style.display = 'flex';
                document.getElementById('quote-client-name').innerText = lead.name;
                document.getElementById('quote-date').innerText = new Date().toLocaleDateString();
                document.getElementById('quote-product').innerText = lead.model;
                document.getElementById('quote-price').innerText = formatCurrency(lead.budget);
                document.getElementById('quote-total').innerText = formatCurrency(lead.budget);
                document.getElementById('quote-final-total').innerText = formatCurrency(lead.budget);
                document.getElementById('quote-obs').innerText = lead.observations || "Sin observaciones.";
            }
        }
        function downloadQuotePDF() {
            showToast("Descargando PDF...", "info");
            setTimeout(() => showToast("PDF descargado (Simulado)", "success"), 1000);
        }

        // --- IMPORT (SIMPLIFIED) ---
        function setupFileImport() {
            const dropZone = document.getElementById('drop-zone');
            const fileInput = document.getElementById('file-input');
            dropZone.addEventListener('dragover', (e) => { e.preventDefault(); dropZone.style.backgroundColor = '#e1e8fa'; });
            dropZone.addEventListener('dragleave', (e) => { e.preventDefault(); dropZone.style.backgroundColor = '#f0f4ff'; });
            dropZone.addEventListener('drop', (e) => {
                e.preventDefault(); dropZone.style.backgroundColor = '#f0f4ff';
                if(e.dataTransfer.files.length) processFile(e.dataTransfer.files[0]);
            });
            fileInput.addEventListener('change', (e) => { if(e.target.files.length) processFile(e.target.files[0]); });
        }
        function processFile(file) {
            showToast("Procesando archivo...", "info");
            setTimeout(() => {
                showToast("Importación simulada exitosa", "success");
            }, 1000);
        }

        function toggleAutomation(key, isActive) {
            automations[key] = isActive;
            showToast(`Automatización ${isActive ? 'activada' : 'desactivada'}`, 'info');
        }

        // --- ADVISOR MANAGEMENT ---
        function openAddAdvisorModal() {
            document.getElementById('add-advisor-modal').style.display = 'flex';
        }

        function handleAddAdvisor(e) {
            e.preventDefault();
            const name = document.getElementById('advisor-name').value;
            const goal = parseInt(document.getElementById('advisor-goal').value);
            
            const newAdvisor = {
                id: Date.now(),
                name: name,
                goal: goal,
                color: '#072870'
            };
            advisors.push(newAdvisor);
            
            document.getElementById('add-advisor-modal').style.display = 'none';
            document.getElementById('advisor-name').value = "";
            document.getElementById('advisor-goal').value = "";
            
            updateAdvisorSelect(); // Actualizar lista en formulario de leads
            renderAdvisors();
            showToast("Asesor agregado", "success");
        }

        function deleteAdvisor(id) {
            if(confirm("¿Eliminar asesor? Los leads asignados quedarán sin asesor.")) {
                advisors = advisors.filter(a => a.id !== id);
                leads.forEach(l => {
                    // Si el lead estaba asignado a este asesor (por nombre, no ID en lead para MVP simple), limpiar
                    if(advisors.find(a => a.id === id).name === l.advisor) l.advisor = null;
                });
                updateAdvisorSelect();
                renderAdvisors();
                showToast("Asesor eliminado", "info");
            }
        }

        function editAdvisorGoal(id) {
            const advisor = advisors.find(a => a.id === id);
            const newGoal = prompt("Nueva Meta Mensual (COP):", advisor.goal);
            if(newGoal && !isNaN(newGoal)) {
                advisor.goal = parseInt(newGoal);
                renderAdvisors();
                showToast("Meta actualizada", "success");
            }
        }

        // --- UTILS ---
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = 'toast';
            let icon = 'ph-info';
            if(type === 'success') { toast.style.borderLeftColor = 'var(--success)'; icon = 'ph-check-circle'; }
            toast.innerHTML = `<i class="ph ${icon}" style="font-size:1.2rem;"></i> <span>${message}</span>`;
            container.appendChild(toast);
            setTimeout(() => { toast.style.opacity = '0'; toast.style.transform = 'translateX(100%)'; setTimeout(() => toast.remove(), 300); }, 4000);
        }

        init();
    </script>
</body>
</html>
