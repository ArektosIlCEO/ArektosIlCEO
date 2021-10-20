using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.IO;
using System.Security.Cryptography.X509Certificates;
using System.Net.Http.Headers;

namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        List<string> date;
        List<int> quantita;
        public Form1()
        {
            date = new List<string>();
            quantita = new List<int>();
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            string riga = "";
            string[] dati;
            
            if (File.Exists("pezzi.txt"))
                using (StreamReader sr = new StreamReader("pezzi.txt"))
                {
                    while (!sr.EndOfStream)
                    {
                        riga = sr.ReadLine();

                        if (!string.IsNullOrEmpty(riga))
                        {
                            dati = riga.Split(',');
                            lstGiorni.Items.Add(Scriviformato(dati[0]) + " " + dati[1]);

                            date.Add(dati[0]);
                            quantita.Add(Convert.ToInt32(dati[1]));
                        }
                    }
                } 
         }
        private string Scriviformato(string dati) {
            // ggmmaaaa
            string gg = dati.Substring(0, 2);
            string mm = dati.Substring(2, 2);
            string aa = dati.Substring(4, 4);

            switch (mm)
            {
                case "01":
                    mm = "Gennaio";
                    break;
                case "02":
                    mm = "Febbraio";
                    break;
                case "03":
                    mm = "Marzo";
                    break;
                case "04":
                    mm = "Aprile";
                    break;
                case "05":
                    mm = "Maggio";
                    break;
                case "06":
                    mm = "Giugno";
                    break;
                case "07":
                    mm = "Luglio";
                    break;
                case "08":
                    mm = "Agosto";
                    break;
                case "09":
                    mm = "Settembre";
                    break;
                case "10":
                    mm = "Ottobre";
                    break;
                case "11":
                    mm = "Novembre";
                    break;
                default:
                    mm = "Dicembre";
                    break;
            }
            
            return gg+ " " + mm+ " " + aa;
        }

        private void button1_Click(object sender, EventArgs e)
        {
            int somma = 0;
            foreach (var el in quantita)
                somma += el;

            lblrisultato.Text = ((float)somma / quantita.Count).ToString();
            panel1.Visible = true;
            panel2.Visible = false;
        }
        private int calcolamax() {
              int max = 0;
              foreach (var el in quantita)
                  if (max < el)
                      max = el;

              return max;
          }
        private void button2_Click(object sender, EventArgs e)
         {
            lsbRisultato.Items.Clear();
            panel1.Visible = false;
            panel2.Visible = true;
            label1.Text = "Produzione massima e':";
            int max = calcolamax();
            for (int i = 0; i < quantita.Count; i++)
                if (quantita[i] == max)
                    lsbRisultato.Items.Add(Scriviformato(date[i]) + "-" + quantita[i]);

        }
        private void button3_Click(object sender, EventArgs e)
        {
            Form2 frm = new Form2(date, quantita);
            frm.ShowDialog();
        }
    }
}
_______________________________________________________________________________________________________________________
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApp1
{
    public partial class Form2 : Form
    {
        List<string> date;
        List<int> quantita;
        public Form2()
        {
            InitializeComponent();
        }
        public Form2(List<string> date, List<int> quantita)
        {
            InitializeComponent();
            this.date = date;
            this.quantita = quantita;
        }

        private void BubbleSort(List<string> date,List<int> a)     //Bubble Sort
        {
            bool scambio = true;
            int quantitatemp;
            string datetemp;
            int posizione = a.Count;
            for (int i = 0; scambio && i < posizione - 1; i++)
            {
                scambio = false;
                for (int j = 0; j < posizione - 1 - i; j++)
                {
                    if (a[j] > a[j + 1])
                    {
                        quantitatemp = a[j];
                        a[j] = a[j + 1];
                        a[j + 1] = quantitatemp;

                        datetemp = date[j];
                        date[j] = date[j + 1];
                        date[j + 1] = datetemp;
                        scambio = true;
                    }
                }
            }
        }
        private string Scriviformato(string data)
        {
            // ggmmaaaa
            string gg = data.Substring(0, 2);
            string mm = data.Substring(2, 2);
            string aa = data.Substring(4, 4);

            switch (mm)
            {
                case "01":
                    mm = "Gennaio";
                    break;
                case "02":
                    mm = "Febbraio";
                    break;
                case "03":
                    mm = "Marzo";
                    break;
                case "04":
                    mm = "Aprile";
                    break;
                case "05":
                    mm = "Maggio";
                    break;
                case "06":
                    mm = "Giugno";
                    break;
                case "07":
                    mm = "Luglio";
                    break;
                case "08":
                    mm = "Agosto";
                    break;
                case "09":
                    mm = "Settembre";
                    break;
                case "10":
                    mm = "Ottobre";
                    break;
                case "11":
                    mm = "Novembre";
                    break;
                default:
                    mm = "Dicembre";
                    break;
            }

            return gg + " " + mm + " " + aa;
        }

        private void Form2_Load(object sender, EventArgs e)
        {
            BubbleSort(date, quantita);
            for(int i=0;i<date.Count;i++)
                listBox1.Items.Add(Scriviformato(date[i]) + " " + quantita[i].ToString());
        }

        private void button1_Click(object sender, EventArgs e)
        {
            this.Close();
        }
    }
}
