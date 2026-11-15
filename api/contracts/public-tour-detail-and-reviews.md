# Public Tour Detail and Reviews

TM-210 and TM-211 implement Report 3 §§3.5.3 and 3.7.2, BR-54/56/59/60 and BR-91–96.

`GET /api/v1/tours/{tourId}` is public. It returns a detail DTO for a published tour with an eligible operator. Missing and nonpublic tours return the same 404. IDs are decimal strings. Schedule availability reflects the read snapshot and does not reserve seats.

| Response field | Source |
|---|---|
| Identity, title, destination, duration, price, description | Published tour record |
| Gallery | Active Tour Media, with the designated primary first |
| Itinerary and inclusions | Existing tour detail records |
| Departure schedules and remaining seats | Schedule inventory at read time |
| Operator | Approved public operator projection |
| `rating.average`, `rating.count` | Confirmed Published Tour TripReviews. When count is zero, average is null. |
| `reviews` | Array of confirmed Published reviews for this tour, newest first with review ID breaking ties |

Each review contains `reviewId`, `displayName`, `rating`, `title`, `content`, `createdAtUtc` and `updatedAtUtc`. Display names follow the review author's stored privacy setting. The neutral name is used where disclosure was declined. The DTO excludes account and booking identifiers and moderation metadata. Legacy review rows without publication status do not contribute. Search continues to return a compact aggregate and the nullable `thumbnailUrl`, without review bodies.

The Booking and Review write APIs retain ownership of completion, eligibility, screening, uniqueness, and editing rules.
