# Predictive tracking

Ron Azuma showed with user testing that even with true acceleration and angular rotation measurements, prediction starts making things worse after 60ms. (_per Russ Taylor, citation needed_)

> Russ Taylor, personal conversation: The two things to look out for with the predictor are: (1) overshoot and bounce-back when the user stops moving their head (predicting too far ahead), and (2) Jittery behavior even when sitting still (too much noise in the tracking system for dead-reckoning prediction without a Kalman or other filter).

# Effect of latency

Rich Holloway's "For a standard task, you get about 1mm of position offset for every 1ms of tracker latency." (_per Russ Taylor, citation needed_)