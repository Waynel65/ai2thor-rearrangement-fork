#### command to run pre-trained model

export CURRENT_TIME=$(date '+%Y-%m-%d_%H-%M-%S') # This is just to record when you ran this inference

```
allenact baseline_configs/one_phase/one_phase_rgb_clipresnet50_dagger.py \
-c pretrained_model_ckpts/exp_OnePhaseRGBClipResNet50Dagger_40proc__stage_00__steps_000065083050.pt \
--extra_tag $CURRENT_TIME \
--eval
```


#### Cloud-rendering
- can be set in `one_phase_rgb_base`
- first need to do `from ai2thor.platform import CloudRendering`
- then: 
```
    return RearrangeTaskSampler.from_fixed_dataset(
            run_walkthrough_phase=False,
            run_unshuffle_phase=True,
            stage=stage,
            allowed_scenes=allowed_scenes,
            scene_to_allowed_rearrange_inds=scene_to_allowed_rearrange_inds,
            rearrange_env_kwargs=dict(
                force_cache_reset=force_cache_reset,
                **cls.REARRANGE_ENV_KWARGS,
                controller_kwargs={
                    "x_display": x_display,
                    **cls.THOR_CONTROLLER_KWARGS,
                    **(
                        {} if thor_controller_kwargs is None else thor_controller_kwargs
                    ),
                    "renderDepthImage": any(
                        isinstance(s, DepthSensor) for s in sensors
                    ),
                    "platform": CloudRendering
                },
            ),
            seed=seed,
            sensors=SensorSuite(sensors),
            max_steps=cls.MAX_STEPS,
            discrete_actions=cls.actions(),
            require_done_action=cls.REQUIRE_DONE_ACTION,
            force_axis_aligned_start=cls.FORCE_AXIS_ALIGNED_START,
            epochs=epochs,
            **kwargs,
        )
```


adjust number of processes in training, validation and testing:
machineparam



Saving test metrics in experiment_output/metrics/OnePhaseRGBClipResNet50Dagger_40proc/2023-04-05_19-21-42/2023-04-05_20-28-00/metrics__test_2023-04-05_20-28-00.json


i also altered:

  if allowed_rearrange_inds_subset is not None:
            allowed_rearrange_inds_subset = tuple(allowed_rearrange_inds_subset)
            assert stage in ["valid", "train_unseen"]
            scene_to_allowed_rearrange_inds = {
                scene: allowed_rearrange_inds_subset for scene in allowed_scenes
            }
        seed = md5_hash_str_as_int(str(allowed_scenes))

        device = (
            devices[process_ind % len(devices)]
            if devices is not None and len(devices) > 0
            else torch.device("cpu")
        )
        x_display: Optional[str] = None
        # gpu_device: Optional[int] = None
        gpu_device: Optional[int] = device ## wayne: why not just assign to device to it?
        thor_platform: Optional[ai2thor.platform.BaseLinuxPlatform] = None




train model with 
export PYTHONPATH=$PYTHONPATH:$PWD
allenact -o rearrange_out -b . baseline_configs/one_phase/one_phase_rgb_clipresnet50_dagger.py