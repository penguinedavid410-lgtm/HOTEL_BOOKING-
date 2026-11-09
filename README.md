# Portfolio Project 2, Part 1 — Hotel Booking Chatbot Intent Classifier

**Repository:** `gumballdavid410-a11y/HOTEL_BOOKING-`  
**Course:** AITCD001 Chatbots Development  
**Status:** Provisional experiment; original Portfolio Project 1 rules and Assignment 8 feedback have not been verified.

## Files in this folder

- `Portfolio_Project_2_Part_1_Classifier.ipynb` — executed notebook following the lecturer's Part 1 sequence.
- `intents.csv` — 180 labelled messages with a stratified 80/20 `split` column.
- `intent_classifier.joblib` — saved character-TF-IDF and Logistic Regression pipeline.
- `README.md` — this report.

## Dataset and split

The CSV contains 180 messages across six intents, with 30 messages per intent. The split is stratified with `random_state=42`:

- Training: 144 messages (24 per intent).
- Test: 36 messages (6 per intent).

**Data limitation:** every row is marked `synthetic` (180 synthetic, 0 genuine user messages). The dataset has not been improved against the lecturer's written Assignment 8 feedback because that feedback document was not present among the available files. The messages are therefore a provisional prototype, not a completed verified Assignment 8 revision.

## The classifier

Both models were trained only on the 144 training messages and evaluated on the same 36 test messages.

| Features | Test accuracy |
|---|---:|
| Word TF-IDF | 80.6% |
| Character TF-IDF (`char_wb`, n-grams 2–4) | **86.1%** |

The character model was kept. Its test classification report is:

| Intent | Precision | Recall | F1 | Test support |
|---|---:|---:|---:|---:|
| `booking_cancellation` | 1.000 | 0.833 | 0.909 | 6 |
| `booking_modification` | 0.833 | 0.833 | 0.833 | 6 |
| `hotel_amenities` | 0.857 | 1.000 | 0.923 | 6 |
| `out_of_scope` | 0.800 | 0.667 | 0.727 | 6 |
| `room_availability` | 1.000 | 1.000 | 1.000 | 6 |
| `room_booking` | 0.714 | 0.833 | 0.769 | 6 |
| **Macro average** | **0.867** | **0.861** | **0.860** | **36** |

### Confusion matrix

Rows are actual labels; columns are predicted labels.

| Actual \ Predicted | Cancellation | Modification | Amenities | Out of scope | Availability | Booking |
|---|---:|---:|---:|---:|---:|---:|
| Cancellation | 5 | 1 | 0 | 0 | 0 | 0 |
| Modification | 0 | 5 | 0 | 0 | 0 | 1 |
| Amenities | 0 | 0 | 6 | 0 | 0 | 0 |
| Out of scope | 0 | 0 | 1 | 4 | 0 | 1 |
| Availability | 0 | 0 | 0 | 0 | 6 | 0 |
| Booking | 0 | 0 | 0 | 1 | 0 | 5 |

## Rules compared with classifier on the same test set

The available Assignment 8 notebook contains five **provisional** keyword rules. The original Portfolio Project 1 source code was not available for verification, so this is not a comparison against the original chatbot's verified rules.

| System | Right | Missed (sent to fallback) | Confident and wrong |
|---|---:|---:|---:|
| Provisional keyword rules | 32 | 1 | 3 |
| Character classifier at threshold 0.30 | 20 | 16 | 0 |

At threshold 0.30, the classifier sends 22 of 36 messages to the fallback (61.1%). Of those, 16 are in-scope requests that it misses; the other six are correctly out-of-scope. It makes no confident wrong predictions on this small test set. The provisional rules have higher accuracy and answer more of the in-scope messages, so the classifier does **not** outperform the rules in this experiment.

## Threshold decision

I selected **0.30** because an incorrect hotel booking answer could give a guest the wrong reservation details. A fallback is inconvenient, but safer. This threshold produced zero confident wrong predictions on this test set, at the cost of a 61.1% fallback rate. This is a conservative trade-off, not evidence that the classifier is better than the rules.

## What remains to be improved

1. Apply the actual written feedback from Assignment 8 and regenerate the dataset.
2. Add at least 20 genuine, anonymised messages collected appropriately; the current 180 messages are all synthetic.
3. Use actual UNMATCHED log and Step 10 local-phrasing examples where available.
4. Verify the intent labels and paste the original Portfolio Project 1 chatbot's actual rules into Step 2, then rerun the same-test-set comparison.
5. Review Kinyarwanda and mixed-language wording with fluent speakers and add more varied examples, especially for the lower-scoring `out_of_scope` and `room_booking` intents.

**Integrity note:** the results in this folder are a reproducible provisional experiment on the synthetic dataset and provisional rules. They are not a verified evaluation of the original Portfolio Project 1 chatbot or an assertion that the Assignment 8 feedback requirements have been met.
