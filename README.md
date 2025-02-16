# -1.1
код
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace calculator
{
    public partial class Form1 : Form
    {
        private double result = 0;
        private string operation = "";
        private bool isOperationPerformed = false;
        private string currentExpression = ""; // Переменная для хранения текущего выражения

        public Form1()
        {
            InitializeComponent();
        }

        private void buttonC_Click(object sender, EventArgs e)
        {
            textBox2.Text = "0";
            currentExpression = ""; // Очистить текущее выражение
            result = 0;
            isOperationPerformed = false;
        }

        private void button_Click(object sender, EventArgs e)
        {
            if ((textBox2.Text == "0") || (isOperationPerformed))
                textBox2.Clear();

            isOperationPerformed = false;
            Button button = (Button)sender;
            textBox2.Text = textBox2.Text + button.Text;

            // Добавить ввод в текущее выражение
            currentExpression += button.Text;
        }

        private void operator_Click(object sender, EventArgs e)
        {
            Button button = (Button)sender;
            operation = button.Text;
            result = Double.Parse(textBox2.Text);
            isOperationPerformed = true;

            // Добавить операцию в текущее выражение
            currentExpression += " " + operation + " ";
            textBox2.Clear(); // Очистить textBox2 для ввода следующего числа
        }

        private void buttonEquals_Click(object sender, EventArgs e)
        {
            // Получить последнее число из currentExpression
            string[] parts = currentExpression.Split(new[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);
            string lastNumber = parts[parts.Length - 1];

            double secondOperand;
            if (!Double.TryParse(lastNumber, out secondOperand))
            {
                textBox2.Text = "Ошибка";
                return; // Прервать выполнение
            }

            double finalResult = 0;

            switch (operation)
            {
                case "+":
                    finalResult = result + secondOperand;
                    break;
                case "-":
                    finalResult = result - secondOperand;
                    break;
                case "*":
                    finalResult = result * secondOperand;
                    break;
                case "/":
                    if (secondOperand != 0)
                        finalResult = result / secondOperand;
                    else
                    {
                        textBox2.Text = "Ошибка";
                        return; // Прервать выполнение
                    }
                    break;
                default:
                    break;
            }

            // Добавить результат в текущее выражение
            currentExpression += " = " + finalResult.ToString();
            textBox2.Text = currentExpression; // Отобразить результат

            // Сбросить операцию
            operation = "";

            // Подготовка для нового ввода
            result = finalResult;
            isOperationPerformed = true;

            // Очистить выражение для нового ввода
            currentExpression = finalResult.ToString();
        }
    }
}
