# Blue Green Deployments

The master is blue, the branch is green.

Deploy from OSEv3.

## new project and blue app from master

    oc new-project bluegreen --display-name="Blue Green" --description='Blue Green Deployments'
    oc new-app https://github.com/foundjem/blue-green.git#master --name=blue --strategy=source

## expose bluegreen service (using blue)

    oc expose service blue --name=bluegreen

## green app deploy

    oc new-app https://github.com/foundjem/blue-green.git#green --name=green

## switch services to green

    oc get route/bluegreen -o yaml | sed -e 's/name: blue$/name: green/' | oc replace -f -

## and back again

    oc get route/bluegreen -o yaml | sed -e 's/name: green$/name: blue/' | oc replace -f -
