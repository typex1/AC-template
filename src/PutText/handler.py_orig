import json
import os
import boto3

# read table name from environment variable
TABLE_NAME = os.environ['TEXTTABLE_NAME']
# create dynamodb resource
dynamodb = boto3.resource('dynamodb')
# create table object
table = dynamodb.Table(TABLE_NAME)

# perform sentiment analysis
def sentiment_analysis(text):
    comprehend = boto3.client(service_name='comprehend')
    sentiment = comprehend.detect_sentiment(Text=text, LanguageCode='en')
    return sentiment['Sentiment']

def handler(event, context):
    # Log the event argument for debugging and for use in local development.
    print(json.dumps(event))
    
    # extract the body from the event json data
    body = json.loads(event['body'])
    # extract the text from the body
    text = body['text']
    # perform sentiment analysis
    sentiment = sentiment_analysis(text)
    # print sentiment
    print("sentiment: {}".format(sentiment))
    # create a new item in the dynamodb table
    result = table.put_item(
        Item={
            'text': text,
            'sentiment': sentiment
        }
    )
    # print result
    print("result: {}".format(result))

    # return sentiment
    return {
        'statusCode': 200,
        'body': json.dumps({
            'sentiment': sentiment
        })
    }
