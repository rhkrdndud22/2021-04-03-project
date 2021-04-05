# 2021-04-03-project
c# 성적표 프로그램

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Xml;

namespace _2021_04_01_project
{
    public partial class Form1 : Form
    {
        private List<Student> liststudents = new List<Student>();
        private List<Subject> listsubjects = new List<Subject>();
        public Form1()
        {
            InitializeComponent();
            Read_Student();
            Read_Subject();

        }
        private void button1_Click_1(object sender, EventArgs e)
        {

            Make_Students();
            Make_Subjects();
        }


        private void Make_Students()
        {
            string studentsOutput = "";
            studentsOutput += "<students>\n";
            foreach (var item in liststudents)
            {
                studentsOutput += "<student>\n";
                studentsOutput += "  <id>" + item.Id + "</id>\n";
                studentsOutput += "  <name>" + item.Name + "</name>\n";
                studentsOutput += "</student>\n";
            }
            studentsOutput += "</students>";



            File.WriteAllText(@"./Students.xml", studentsOutput);
        }

        private void Make_Subjects()
        {
            string subjectsOutput = "";
            subjectsOutput += "<subjects>\n";
            foreach (var item in listsubjects)
            {
                subjectsOutput += "<subject>\n";
                subjectsOutput += "  <korean>" + item.korean + "</korean>\n";
                subjectsOutput += "  <english>" + item.English + "</english>\n";
                subjectsOutput += "  <math>" + item.Math + "</math>\n";
                subjectsOutput += "  <society>" + item.Society + "</society>\n";
                subjectsOutput += "  <science>" + item.Science + "</science>\n";
                subjectsOutput += " <sum> " + item.Sum + "</sum>\n";
                subjectsOutput += " <avarage>" + item.Avarage + "</avarage>";
                subjectsOutput += "</subject>\n";
            }
            subjectsOutput += "</subjects>";



            File.WriteAllText(@"./Subjects.xml", subjectsOutput);
        }
        private void button2_Click_1(object sender, EventArgs e)
        {
            Read_Student();
            Read_Subject();

        }


        private void Read_Student()
        {
            try
            {
                FileInfo fib = new FileInfo(@"./students.xml");

                if (!fib.Exists)
                {
                    MessageBox.Show("students.xml 파일이 존재하지 않습니다.");
                    return;
                }

                liststudents.Clear();

                string studentsOutput = File.ReadAllText(@"./students.xml");
                XmlDocument xdocStudent = new XmlDocument();
                xdocStudent.Load(@"./students.xml");
                XmlNodeList nodeStudents = xdocStudent.SelectNodes("/students/student");
                foreach (XmlNode student in nodeStudents)
                {
                    liststudents.Add(new Student()
                    {
                        Id = int.Parse(student.SelectSingleNode("id").InnerText),
                        Name = student.SelectSingleNode("name").InnerText,

                    });
                }
            }
            catch (FileLoadException exception)
            {
                // 파일이 없으면 예외 발생: 새로운 파일 생성
                Make_Students();
            }

        }
        private void Read_Subject()
        {
            try
            {
                FileInfo fib = new FileInfo(@"./subjects.xml");

                if (!fib.Exists)
                {
                    MessageBox.Show("subjects.xml 파일이 존재하지 않습니다.");
                    return;
                }

                listsubjects.Clear();

                string subjectsOutput = File.ReadAllText(@"./subjects.xml");
                XmlDocument xdocsubject = new XmlDocument();
                xdocsubject.Load(@"./subjects.xml");
                XmlNodeList nodSubjects = xdocsubject.SelectNodes("/subjects/subject");
                foreach (XmlNode subject in nodSubjects)
                {
                    listsubjects.Add(new Subject()
                    {
                        
                        
                        korean = subject.SelectSingleNode("korean").InnerText,
                        English= subject.SelectSingleNode("english").InnerText,
                        Math= subject.SelectSingleNode("math").InnerText,
                        Society= subject.SelectSingleNode("society").InnerText,
                        Science= subject.SelectSingleNode("science").InnerText,
                        Sum = int.Parse(subject.SelectSingleNode("sum").InnerText),
                        Avarage = int.Parse(subject.SelectSingleNode("avarage").InnerText)
                    });
                }
            }
            catch (FileLoadException exception)
            {
                // 파일이 없으면 예외 발생: 새로운 파일 생성
                Make_Subjects();
            }
        }
        private void button3_Click_1(object sender, EventArgs e)
        {
            dataGridView1.DataSource = null;
            dataGridView1.DataSource = liststudents;
            dataGridView2.DataSource = null;
            dataGridView2.DataSource = listsubjects;

        }




        private void button4_Click_1(object sender, EventArgs e)
        {
            if (textBox1.Text != " " || textBox2.Text != " ")
            {
                int output;
                if (int.TryParse(textBox1.Text, out output))
                    liststudents.Add(new Student()
                    {
                        Id = output,
                        Name = textBox2.Text
                    });

                dataGridView1.DataSource = null;
                dataGridView1.DataSource = liststudents;
            }

        }
        private void button5_Click(object sender, EventArgs e)
        {
            if (textBox3.Text != " " || textBox4.Text != " ")
            {
                int output;
                int output1;
                int output2;
                int output3;
                int output4;
                
                if (int.TryParse(textBox3.Text, out output))
                if (int.TryParse(textBox4.Text, out output1))
                if (int.TryParse(textBox5.Text, out output2))
                if (int.TryParse(textBox6.Text, out output3))
                if (int.TryParse(textBox7.Text, out output4))
                                listsubjects.Add(new Subject()
                    {
                        korean = textBox3.Text,
                        English = textBox4.Text,
                        Math = textBox5.Text,
                        Society = textBox6.Text,
                        Science = textBox7.Text,
                        Avarage = (output+output1+output2+output3+output4)/5,
                        Sum = (output + output1 + output2 + output3 + output4)


                                });

              

                

                dataGridView1.DataSource = null;
                dataGridView1.DataSource = liststudents;



            }
        }
    }
}
