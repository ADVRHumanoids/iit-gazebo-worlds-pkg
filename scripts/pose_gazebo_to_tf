#!/usr/bin/env python
import rospy, tf2_ros
import argparse, sys
import copy
from gazebo_msgs.msg import ModelStates
from geometry_msgs.msg import Pose, TransformStamped


parser = argparse.ArgumentParser(description='This ROS node subscribe to "/gazebo/model_states", read the Pose of the "models" and publish the according tf (eg for rviz) from the "reference" link to the "models_links". Note that for fixed transforms (ie fixed joint) it is better to use the robot state publisher')
parser._action_groups.pop()
required = parser.add_argument_group('required arguments')
optional = parser.add_argument_group('optional arguments')
optional.add_argument('--reference', help='Reference wrt the tf will be published', default="world")
optional.add_argument('--rate', help='Rate of tf pub', default=100, type=float)
required.add_argument('--models', nargs='+', required=True)
required.add_argument('--models_link', nargs='+', required=True)
    #nargs + : 1 or more argument in succession
args, unknown = parser.parse_known_args()

reference_link = args.reference
models = args.models
models_link = args.models_link

tf_broadcaster = tf2_ros.TransformBroadcaster()
tf_msgs = {}

new_msg = False
lastUpdateTime = None
updatePeriod = 1/args.rate

def pose_callback(msg):
    
    global new_msg
    global lastUpdateTime
    time = rospy.Time.now()

    sinceLastUpdateDuration = time - lastUpdateTime
    #rospy.loginfo("time: %s, last: %s, diff:%s, max:%s", time, lastUpdateTime, sinceLastUpdateDuration, updatePeriod)            
    if sinceLastUpdateDuration.to_sec() < updatePeriod:
        return
        
    lastUpdateTime = time
    
    n_model_count = 0 #if less than the model given as arg, do not publish tf at all
    
    for i in range(len(msg.name)):
        
        #rospy.loginfo("Get %s ", msg.name[i])            

        val = tf_msgs.get(msg.name[i])
        
        if val != None :
        
            val.header.stamp = time
            val.transform.translation = msg.pose[i].position
            val.transform.rotation = msg.pose[i].orientation
            
            #rospy.loginfo("%s : %d %d %d, %d %d %d %d", msg.name[i], val.transform.translation.x, val.transform.translation.y, 
            #             val.transform.translation.z,val.transform.rotation.x, val.transform.rotation.y, val.transform.rotation.z, val.transform.rotation.w)            

            n_model_count = n_model_count+1
            
        #else :
            #rospy.loginfo("Get %s model which is not requested by caller with 'models' argument ", msg.name[i])  
            
        if n_model_count == len(models):
            new_msg = True
     

if __name__ == '__main__':
    
    rospy.init_node('pose_gazebo_to_rviz', anonymous=False)
    
    if len(models) != len(models_link):
        rospy.logerr("Please pass same number of models and models_link, you passed %d %d", len(models), len(models_link))
        sys.exit(-1)
    
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.
    
    for i in range(len(models)):
        tf_msgs[models[i]] = TransformStamped()
        tf_msgs[models[i]].header.frame_id = reference_link
        tf_msgs[models[i]].child_frame_id = models_link[i]
        tf_msgs[models[i]].transform.translation.x = 0
        tf_msgs[models[i]].transform.translation.y = 0
        tf_msgs[models[i]].transform.translation.z = 0
        tf_msgs[models[i]].transform.rotation.x = 0
        tf_msgs[models[i]].transform.rotation.y = 0
        tf_msgs[models[i]].transform.rotation.z = 0
        tf_msgs[models[i]].transform.rotation.w = 1
        
    lastUpdateTime = rospy.get_rostime()

    rospy.Subscriber("/gazebo/model_states", ModelStates, pose_callback)

    #in python spinOnce does not exist, it is not necessary
    rate = rospy.Rate(args.rate)
    
    rospy.loginfo("waiting gazebo...")

    while (not new_msg) and (not rospy.is_shutdown()):

        rate.sleep()
    
    
    rospy.loginfo("publishing tf...")
    
    while not rospy.is_shutdown():
        
        if new_msg:
            local_tfs = copy.deepcopy(tf_msgs) #to avoid race conditions?
            
            for tf_msg in local_tfs.values():
                tf_broadcaster.sendTransform(tf_msg)
            
            new_msg = False


        rate.sleep()

