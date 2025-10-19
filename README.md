<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trò Chơi Hóc Búa - by trungquan</title>
    
    <style>
        /* CSS nhúng trực tiếp */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f4f9;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        .container {
            background-color: #ffffff;
            padding: 30px 40px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
        }

        h1 {
            color: #007bff;
            text-align: center;
            margin-bottom: 5px;
        }

        .creator {
            text-align: center;
            font-style: italic;
            color: #666;
            margin-bottom: 25px;
        }

        .question-block {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            background-color: #fafafa;
        }

        .question-text {
            font-weight: bold;
            margin-bottom: 15px;
            font-size: 1.1em;
            color: #333;
        }

        .answer-option {
            display: block;
            margin-bottom: 10px;
            cursor: pointer;
            padding: 8px;
            border-radius: 4px;
            transition: background-color 0.2s;
        }

        .answer-option:hover {
            background-color: #e9e9e9;
        }

        input[type="radio"] {
            margin-right: 10px;
        }

        #submit-btn {
            display: block;
            width: 100%;
            padding: 12px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 6px;
            font-size: 1.1em;
            cursor: pointer;
            margin-top: 20px;
            transition: background-color 0.3s;
        }

        #submit-btn:hover {
            background-color: #218838;
        }

        #result {
            margin-top: 25px;
            padding: 20px;
            border: 2px solid #007bff;
            border-radius: 8px;
            text-align: center;
            background-color: #eaf6ff;
        }

        .hidden {
            display: none;
        }

        /* Hiệu ứng khi có đáp án */
        .correct {
            background-color: #d4edda !important; /* Dùng !important để ghi đè hover */
            border-color: #c3e6cb;
        }

        .incorrect {
            background-color: #f8d7da !important;
            border-color: #f5c6cb;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Trò Chơi Hóc Búa</h1>
        <p class="creator">by **trungquan**</p>

        <div id="quiz-container">
            </div>

        <button id="submit-btn" onclick="checkAnswers()">Kiểm Tra Đáp Án</button>

        <div id="result" class="hidden">
            <h2>Kết Quả:</h2>
            <p id="score-display"></p>
            <p id="message"></p>
        </div>
    </div>

    <script>
        // JavaScript nhúng trực tiếp
        const questions = [
            {
                question: "Cái gì có cổ mà không có đầu, có chân mà không có giày?",
                options: ["Cái bàn", "Cái ghế", "Cái áo", "Cái giường"],
                answer: "Cái áo"
            },
            {
                question: "Một người xây nhà có một viên gạch bị bể, ông ta làm cách nào để xây xong ngôi nhà mà không cần mua thêm gạch?",
                options: ["Lật viên gạch lại", "Ném viên gạch đi", "Dùng viên gạch đó làm móng", "Viên gạch đó là viên gạch cuối cùng"],
                answer: "Dùng viên gạch đó làm móng" 
            },
            {
                question: "Cái gì càng lớn thì càng nhẹ?",
                options: ["Quả bóng bay", "Cái lỗ", "Bông gòn", "Hòn đá"],
                answer: "Cái lỗ"
            }
        ];

        const quizContainer = document.getElementById('quiz-container');
        const resultDiv = document.getElementById('result');
        const scoreDisplay = document.getElementById('score-display');
        const messageDisplay = document.getElementById('message');

        // Hàm hiển thị câu hỏi
        function renderQuiz() {
            questions.forEach((q, index) => {
                const block = document.createElement('div');
                block.className = 'question-block';

                const questionText = document.createElement('p');
                questionText.className = 'question-text';
                questionText.textContent = `${index + 1}. ${q.question}`;
                block.appendChild(questionText);

                q.options.forEach(option => {
                    const label = document.createElement('label');
                    label.className = 'answer-option';
                    
                    const input = document.createElement('input');
                    input.type = 'radio';
                    input.name = `question${index}`;
                    input.value = option;

                    label.appendChild(input);
                    label.appendChild(document.createTextNode(option));
                    block.appendChild(label);
                });

                quizContainer.appendChild(block);
            });
        }

        // Hàm kiểm tra đáp án
        window.checkAnswers = function() {
            let score = 0;
            const totalQuestions = questions.length;

            // Reset kết quả
            resultDiv.classList.add('hidden');
            
            // Loại bỏ các lớp màu trước đó
            document.querySelectorAll('.answer-option').forEach(label => {
                label.classList.remove('correct', 'incorrect');
            });

            questions.forEach((q, index) => {
                const selectedAnswer = document.querySelector(`input[name="question${index}"]:checked`);
                let isCorrect = false;

                if (selectedAnswer) {
                    const selectedValue = selectedAnswer.value;
                    if (selectedValue === q.answer) {
                        score++;
                        isCorrect = true;
                    }

                    // Đánh dấu đáp án của người dùng
                    selectedAnswer.parentElement.classList.add(isCorrect ? 'correct' : 'incorrect');
                }

                // Đánh dấu đáp án đúng (cho người chơi thấy)
                document.querySelectorAll(`input[name="question${index}"]`).forEach(input => {
                    if (input.value === q.answer) {
                        input.parentElement.classList.add('correct');
                    }
                });
            });

            // Hiển thị kết quả
            scoreDisplay.textContent = `Bạn trả lời đúng: ${score}/${totalQuestions}`;
            
            let message = '';
            if (score === totalQuestions) {
                message = '🎉 Chúc mừng! Bạn quá xuất sắc! Đã giải đố thành công tất cả các câu hỏi hóc búa! 🎉';
            } else if (score >= totalQuestions / 2) {
                message = '👍 Khá lắm! Bạn đã trả lời đúng được một số câu hóc búa rồi đấy. Cố gắng thêm nhé!';
            } else {
                message = '🧐 Đừng nản lòng! Đây là những câu hỏi rất hóc búa. Hãy thử lại để xem đáp án nhé!';
            }
            messageDisplay.textContent = message;
            
            resultDiv.classList.remove('hidden');
            resultDiv.scrollIntoView({ behavior: 'smooth' }); // Cuộn xuống để xem kết quả
        }

        // Khởi tạo trò chơi khi trang tải xong
        document.addEventListener('DOMContentLoaded', renderQuiz);
    </script>
</body>
</html>
