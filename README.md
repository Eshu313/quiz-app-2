# quiz-app-2
quiz application
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center"
    android:background="#FFFFFF">

    <!-- Question Text -->
    <TextView
        android:id="@+id/question_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="What is the capital of France?"
        android:textSize="20sp"
        android:textColor="#000000"
        android:layout_marginBottom="30dp"
        android:textAlignment="center"/>

    <!-- Option Buttons -->
    <Button
        android:id="@+id/option1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Option 1" />

    <Button
        android:id="@+id/option2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Option 2" />

    <Button
        android:id="@+id/option3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Option 3" />

    <Button
        android:id="@+id/option4"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Option 4" />

    <!-- Score Display -->
    <TextView
        android:id="@+id/score_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Score: 0"
        android:textSize="16sp"
        android:textColor="#000000"/>

</LinearLayout>

#MAIN ACTIVITY.JAVA
package com.example.quizapp;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    TextView questionText, scoreText;
    Button option1, option2, option3, option4;

    Quiz quiz;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Initialize views
        questionText = findViewById(R.id.question_text);
        scoreText = findViewById(R.id.score_text);
        option1 = findViewById(R.id.option1);
        option2 = findViewById(R.id.option2);
        option3 = findViewById(R.id.option3);
        option4 = findViewById(R.id.option4);

        // Initialize quiz and load first question
        quiz = new Quiz();
        loadQuestion();

        // Set onClickListener for options
        View.OnClickListener listener = v -> {
            Button clicked = (Button) v;
            quiz.checkAnswer(clicked.getText().toString());

            if (quiz.isFinished()) {
                questionText.setText("Quiz Finished! Final Score: " + quiz.getScore());
                option1.setVisibility(View.GONE);
                option2.setVisibility(View.GONE);
                option3.setVisibility(View.GONE);
                option4.setVisibility(View.GONE);
            } else {
                loadQuestion();
            }
        };

        option1.setOnClickListener(listener);
        option2.setOnClickListener(listener);
        option3.setOnClickListener(listener);
        option4.setOnClickListener(listener);
    }

    // Load question and options
    void loadQuestion() {
        questionText.setText(quiz.getQuestion());
        String[] opts = quiz.getOptions();
        option1.setText(opts[0]);
        option2.setText(opts[1]);
        option3.setText(opts[2]);
        option4.setText(opts[3]);
        scoreText.setText("Score: " + quiz.getScore());
    }
}
#JAVA CODE
package com.example.quizapp;

public class Quiz {

    // Questions, options, and answers
    private String[] questions = {
        "What is the capital of France?",
        "Which planet is known as the Red Planet?",
        "Who wrote 'Romeo and Juliet'?"
    };

    private String[][] options = {
        {"Paris", "London", "Berlin", "Madrid"},
        {"Earth", "Mars", "Jupiter", "Venus"},
        {"Shakespeare", "Dickens", "Austen", "Hemingway"}
    };

    private String[] answers = {"Paris", "Mars", "Shakespeare"};

    private int index = 0; // To track the current question
    private int score = 0; // The user's score

    // Get current question
    public String getQuestion() {
        return questions[index];
    }

    // Get options for current question
    public String[] getOptions() {
        return options[index];
    }

    // Check if the answer is correct
    public void checkAnswer(String selectedAnswer) {
        if (selectedAnswer.equals(answers[index])) {
            score++;
        }
        index++; // Move to the next question
    }

    // Check if the quiz is finished
    public boolean isFinished() {
        return index >= questions.length;
    }

    // Get current score
    public int getScore() {
        return score;
    }
}
