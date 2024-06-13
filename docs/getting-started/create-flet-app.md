python -m venv venv
source venv/bin/activate  # Linux/Mac
.\venv\Scripts\activate  # Windows
pip install flask pandas sqlalchemy fpdf matplotlib
from flask import Flask, request, jsonify, render_template
from flask_sqlalchemy import SQLAlchemy

app = Flask(__frequencia superaçao__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///frequencia.db'
db = SQLAlchemy(app)

class Aluno(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    nome = db.Column(db.String(80), nullable=False)
    faltas = db.Column(db.Integer, default=0)
    presencas = db.Column(db.Integer, default=0)

db.create_all()

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/registro', methods=['POST'])
def registro():
    data = request.json
    aluno = Aluno.query.get(data['id'])
    if data['presenca']:
        aluno.presencas += 1
    else:
        aluno.faltas += 1
    db.session.commit()
    return jsonify({'message': 'Registro atualizado'})

@app.route('/relatorio/<int:id>', methods=['GET'])
def relatorio(id):
    aluno = Aluno.query.get(id)
    if aluno:
        total = aluno.faltas + aluno.presencas
        percentual_presenca = (aluno.presencas / total) * 100
        percentual_falta = (aluno.faltas / total) * 100

        # Geração de PDF (usando FPDF)
        from fpdf import FPDF

        class PDF(FPDF):
            def header(self):
                self.set_font('Arial', 'B', 12)
                self.cell(0, 10, 'Relatorio de Frequencia', 0, 1, 'C')

            def chapter_title(self, title):
                self.set_font('Arial', 'B', 12)
                self.cell(0, 10, title, 0, 1, 'L')
                self.ln(10)

            def chapter_body(self, body):
                self.set_font('Arial', '', 12)
                self.multi_cell(0, 10, body)
                self.ln()

        pdf = PDF()
        pdf.add_page()
        pdf.chapter_title(f'Relatorio de Frequencia - {aluno.nome}')
        body = (f'Total de Presenças: {aluno.presencas}\n'
                f'Total de Faltas: {aluno.faltas}\n'
                f'Percentual de Presença: {percentual_presenca:.2f}%\n'
                f'Percentual de Falta: {percentual_falta:.2f}%')
        pdf.chapter_body(body)

        pdf_output = f'relatorio_{aluno.id}.pdf'
        pdf.output(pdf_output)

        return jsonify({'message': 'Relatório gerado', 'file': pdf_output})
    return jsonify({'message': 'Aluno não encontrado'}), 404

if __name__ == '__main__':
    app.run(debug=True)
