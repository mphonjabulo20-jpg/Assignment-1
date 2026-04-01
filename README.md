# Assignment-1 
package com.example.assignment1

import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.TextView
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivity : AppCompatActivity() {

    private lateinit var editTextTimeOfDay: EditText
    private lateinit var buttonGetSuggestion: Button
    private lateinit var textViewSuggestion: TextView
    private lateinit var buttonReset: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_main)
        
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        // Initialize views
        editTextTimeOfDay = findViewById(R.id.editTextTimeOfDay)
        buttonGetSuggestion = findViewById(R.id.buttonGetSuggestion)
        textViewSuggestion = findViewById(R.id.textViewSuggestion)
        buttonReset = findViewById(R.id.buttonReset)

        // Set up onClick listener for Get Suggestion button
        buttonGetSuggestion.setOnClickListener {
            val timeOfDay = editTextTimeOfDay.text.toString().trim().lowercase()

            // Get suggestion based on input time of day
            val suggestion = getSocialSparkSuggestion(timeOfDay)

            if (suggestion.isNotEmpty()) {
                textViewSuggestion.text = suggestion
            } else {
                Toast.makeText(this, getString(R.string.error_invalid_input), Toast.LENGTH_SHORT).show()
            }
        }

        // Set up onClick listener for Reset button
        buttonReset.setOnClickListener {
            editTextTimeOfDay.text.clear()
            textViewSuggestion.text = getString(R.string.default_suggestion)
        }
    }

    // Function to provide a suggestion based on the input time of day
    private fun getSocialSparkSuggestion(timeOfDay: String): String {
        return when (timeOfDay) {
            "morning" -> getString(R.string.suggestion_morning)
            "mid-morning" -> getString(R.string.suggestion_mid_morning)
            "afternoon" -> getString(R.string.suggestion_afternoon)
            "afternoon snack time" -> getString(R.string.suggestion_afternoon_snack)
            "dinner" -> getString(R.string.suggestion_dinner)
            "after dinner", "night" -> getString(R.string.suggestion_night)
            else -> ""  // Return empty if invalid input
        }
    }
}
