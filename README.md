# Codes-java
import java.util.*;
import java.util.concurrent.TimeUnit;

public class QuizApp {
    private static class Question {
        String question;
        String[] options;
        int correctAnswerIndex;

        Question(String question, String[] options, int correctAnswerIndex) {
            this.question = question;
            this.options = options;
            this.correctAnswerIndex = correctAnswerIndex;
        }

        boolean isAnswerCorrect(int answerIndex) {
            return answerIndex == correctAnswerIndex;
        }
    }

    private static final Scanner scanner = new Scanner(System.in);
    private static int score = 0;
    private static List<Question> questions = new ArrayList<>();

    public static void main(String[] args) {
        // Sample questions (you can add more)
        questions.add(new Question("What is the capital of France?", new String[]{"Berlin", "Madrid", "Paris", "Rome"}, 2));
        questions.add(new Question("Which planet is known as the Red Planet?", new String[]{"Earth", "Mars", "Jupiter", "Saturn"}, 1));
        questions.add(new Question("What is 2 + 2?", new String[]{"3", "4", "5", "6"}, 1));

        // Start the quiz
        for (int i = 0; i < questions.size(); i++) {
            askQuestion(questions.get(i), i + 1);
        }

        // Final result
        showResult();
    }

    private static void askQuestion(Question question, int questionNumber) {
        System.out.println("Question " + questionNumber + ": " + question.question);
        displayOptions(question.options);

        // Start timer for the question
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println("\nTime's up!");
                submitAnswer(-1);  // Automatically submit if time's up
            }
        }, 30000);  // 30 seconds for each question

        System.out.print("Select an option (0-3): ");
        int userAnswer = -1;
        try {
            userAnswer = Integer.parseInt(scanner.nextLine());
        } catch (NumberFormatException e) {
            System.out.println("Invalid input. Please enter a number between 0 and 3.");
        }

        timer.cancel();  // Cancel the timer if answer is submitted before time runs out

        if (userAnswer >= 0 && userAnswer <= 3) {
            submitAnswer(userAnswer);
        } else {
            System.out.println("Invalid option selected. No points awarded.");
        }
    }

    private static void displayOptions(String[] options) {
        for (int i = 0; i < options.length; i++) {
            System.out.println(i + ": " + options[i]);
        }
    }

    private static void submitAnswer(int answer) {
        if (answer == -1) {
            System.out.println("No answer submitted.");
        } else {
            Question currentQuestion = questions.get(questions.size() - 1);
            if (currentQuestion.isAnswerCorrect(answer)) {
                System.out.println("Correct!");
                score++;
            } else {
                System.out.println("Incorrect!");
            }
        }
    }

    private static void showResult() {
        System.out.println("\nQuiz finished!");
        System.out.println("Your final score is: " + score + "/" + questions.size());
        for (int i = 0; i < questions.size(); i++) {
            System.out.println("Question " + (i + 1) + ": " + (questions.get(i).isAnswerCorrect(i) ? "Correct" : "Incorrect"));
        }
    }
}
