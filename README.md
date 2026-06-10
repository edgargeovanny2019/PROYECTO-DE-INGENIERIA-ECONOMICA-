% =========================================================================
% TAREA: SIMULADOR DE FINANZAS - INGENIERÍA ECONÓMICA
% PROPUESTA INICIAL GENERADA POR EL ALUMNO: EDGAR GEOVANNY DÍAZ MEZA
% =========================================================================
% Descripción: Programa interactivo para calcular el impacto del interés
% en un pago único a largo plazo y evaluar opciones de financiamiento.
% =========================================================================

clear; clc;

fprintf('=======================================================\n');
fprintf('        PROYECTO DE INGENIERÍA ECONÓMICA\n');
fprintf('  Propuesta Inicial: Edgar Geovanny Díaz Meza\n');
fprintf('=======================================================\n\n');

fprintf('--- CONFIGURACIÓN DEL SISTEMA ---\n');
fprintf('Este programa es flexible. Puedes ingresar los meses y las tasas\n');
fprintf('de interés que desees para evaluar cualquier caso.\n\n');

% 1. Solicitud de datos interactivos al usuario
monto_inicial = input('Introduce el pago inicial / Enganche ($): ');
pago_mensual = input('Introduce el pago mensual ($): ');
anios = input('Introduce el plazo en años: ');

% Conversión automática de años a meses para el cálculo
meses = anios * 12;

fprintf('\n--- CONFIGURACIÓN DE TASAS DE INTERÉS ---\n');
fprintf('Introduce las tasas en porcentaje anual (ej. 15 para 15%%)\n');
tasa_A_anual = input('Introduce la tasa de interés anual del Banco A: ');
tasa_B_anual = input('Introduce la tasa de interés anual del Banco B: ');

% 2. Conversión de tasas anuales a mensuales (decimal)
i_A = (tasa_A_anual / 100) / 12;
i_B = (tasa_B_anual / 100) / 12;

% 3. Cálculos de Ingeniería Económica (Valor Futuro)
% Se calcula el VF del enganche y el VF de la serie de pagos mensuales (Anualidad)

% Banco A
vf_enganche_A = monto_inicial * (1 + i_A)^meses;
vf_mensualidades_A = pago_mensual * (((1 + i_A)^meses - 1) / i_A);
pago_unico_A = vf_enganche_A + vf_mensualidades_A;

% Banco B
vf_enganche_B = monto_inicial * (1 + i_B)^meses;
vf_mensualidades_B = pago_mensual * (((1 + i_B)^meses - 1) / i_B);
pago_unico_B = vf_enganche_B + vf_mensualidades_B;

% 4. Despliegue de Resultados obtenidos
fprintf('\n=======================================================\n');
fprintf('               RESULTADOS DEL ANÁLISIS\n');
fprintf('=======================================================\n');
fprintf('Plazo total de evaluación: %d años (%d meses)\n', anios, meses);
fprintf('Enganche aportado: $%.2f\n', monto_inicial);
fprintf('Pago mensual considerado: $%.2f\n\n', pago_mensual);

fprintf('Opción Banco A (%.2f%% Interés Anual):\n', tasa_A_anual);
fprintf('  -> Pago único equivalente al mes %d: $%.2f\n', meses, pago_unico_A);
fprintf('  -> Total de intereses acumulados: $%.2f\n\n', pago_unico_A - (monto_inicial + (pago_mensual * meses)));

fprintf('Opción Banco B (%.2f%% Interés Anual):\n', tasa_B_anual);
fprintf('  -> Pago único equivalente al mes %d: $%.2f\n', meses, pago_unico_B);
fprintf('  -> Total de intereses acumulados: $%.2f\n', pago_unico_B - (monto_inicial + (pago_mensual * meses)));
fprintf('=======================================================\n');

% 5. Criterio de Decisión Económica
if pago_unico_A < pago_unico_B
    ahorro = pago_unico_B - pago_unico_A;
    fprintf('CONSEJO: Te conviene el Banco A. El pago único final es menor por $%.2f\n', ahorro);
elseif pago_unico_B < pago_unico_A
    ahorro = pago_unico_A - pago_unico_B;
    fprintf('CONSEJO: Te conviene el Banco B. El pago único final es menor por $%.2f\n', ahorro);
else
    fprintf('CONSEJO: Ambas opciones financieras son económicamente equivalentes.\n');
end
fprintf('=======================================================\n');

% 6. Simulación gráfica del comportamiento de la deuda/pago
ejex_meses = 0:meses;
progreso_A = zeros(1, length(ejex_meses));
progreso_B = zeros(1, length(ejex_meses));

for m = 0:meses
    if m == 0
        progreso_A(m+1) = monto_inicial;
        progreso_B(m+1) = monto_inicial;
    else
        progreso_A(m+1) = (monto_inicial * (1 + i_A)^m) + (pago_mensual * (((1 + i_A)^m - 1) / i_A));
        progreso_B(m+1) = (monto_inicial * (1 + i_B)^m) + (pago_mensual * (((1 + i_B)^m - 1) / i_B));
    end
end

figure;
plot(ejex_meses, progreso_A, 'r-', 'LineWidth', 2); hold on;
plot(ejex_meses, progreso_B, 'b--', 'LineWidth', 2);
grid on;
title('Análisis de Ingeniería Económica: Crecimiento del Valor Futuro');
xlabel('Tiempo (Meses)');
ylabel('Valor Equivalente Acumulado ($)');
legend(sprintf('Banco A (%.1f%%)', tasa_A_anual), sprintf('Banco B (%.1f%%)', tasa_B_anual), 'Location', 'northwest');
